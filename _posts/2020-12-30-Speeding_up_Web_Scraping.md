---
date: 2020-12-30
tags: parallelism swift tutorial web_scraping
title: Speeding up Web Scraping
---
# Introduction
In this tutorial we'll use Swift concurrency primitives to speed up our
[toy web scraper](https://jackpal.github.io/2020/12/29/Web_Scraping-with-Swift-Soup.html).

| Program Version                 | Time (s) |
| ------------------------------- | -------: |
| Original                        |     17.7 |
| Release                         |      4.7 |
| DispatchQueue.concurrentPerform |      1.1 |
| URLSession                      |      0.6 |

<!--more-->
# Web Scraping is an Embarrassingly Parallel Problem

Our toy web scraping problem can be divided into three steps:

1. Fetch the master web page that contains a list of houseplants.
2. For each houseplant, fetch its web page.
3. Combine the information from the first 2 steps into a data structure, and write that to disk.

The second phase can be done in parallel.

## Measuring performance

Starting with the existing web scraper, let's add a simple time benchmark function:

```swift
func time<Result>(name: String = "",
                     warmup: Bool = false,
                     repeatCount: Int = 1,
                     _ callback:  @autoclosure () throws-> Result)
rethrows-> Result {
  if warmup {
    _ = try callback()
  }

  let start = Date()
  var result: Result?
  for _ in 0..<repeatCount {
    result = try callback()
  }
  print("\(name): \(-start.timeIntervalSinceNow / TimeInterval(repeatCount))")
  return result!
}

let houseplants = try time(name:"Scrape", scrapeHouseplants(url:url))
```

On my machine, it took about 17.73 seconds to run, with a variation of about 0.3 seconds between
runs.

First off, let's turn on optimization. By default Xcode builds projects using non-optimized "Debug"
mode. Editing the active scheme to specify that we run with a Release build configuration
brings the time down to 4.7 seconds.

Using the Instruments tool shows that CPU utilization is low. The process is using just one
thread, and that thread is spending much of its time waiting for the network.

## Parallelising scrapeHouseplants

In order to provide opportunities for parallelism, we need to split our existing scrapeHouseplants
function into three smaller functions:

1. scrapeListOfHouseplants: A function that scrapes the main houseplants page and produces a list
of individual houseplants.
2. reduceHouseplantInfos: A function that combines a list of HouseplantInfos into the final
HouseplantCategoryDictionary
3. scrapeHouseplants: An overall function that coordinates the whole scraping process.

While we're refactoring, we'll also change scrapeHouseplantInfo to pass in the html content as a
string, rather than fetching it inside the scrapeHouseplantInfo function. This separation will come
in handy later, when we want to change the way we fetch data.

```swift
import Foundation
import SwiftSoup

struct HouseplantInfo : Codable {
  var description: String
}

// The key is a houseplant name.
typealias HouseplantInfoDictionary = [String:HouseplantInfo]

// The key is a category name.
typealias HouseplantCategoryDictionary = [String:HouseplantInfoDictionary]

func scrapeHouseplantInfo(url: URL, html: String) throws -> HouseplantInfo {
  let document = try SwiftSoup.parse(html)

  var element : Element?

  let span = try document.select("#Description").first()
    ?? document.select("#Description_and_biology").first()
    ?? document.select("#Name_and_description").first()
    ?? document.select("#Plant_care").first()

  if span != nil {
    let h2 = span!.parent()!
    element = try h2.nextElementSibling()
  } else {
    // Start collecting text from the beginning of the web page.
    let div = try document.select(".mw-parser-output")
    element = div.first()?.children()[3]
  }

  var description = ""
  while element != nil {
    if element!.tagName().starts(with: "h") {
      break
    }
    description += try element!.text()
    element = try element!.nextElementSibling()
  }
  return HouseplantInfo(description: description)
}

// Remove […] or (…) text from a string.
func clean(name: String)-> String {
  name.replacingOccurrences(of: #"(\s*\[.*]|\s*\(.*\))"#, with: "", options: .regularExpression)
}

typealias HouseplantKey = (category:String, name: String)
typealias HouseplantTicket = (key: HouseplantKey, url:URL)

func scrapeListOfHouseplants(url: URL, html:String) throws -> [HouseplantTicket] {
  let document = try SwiftSoup.parse(html)

  let span = try document.select("#List_of_common_houseplants").first()!
  let h2 = span.parent()!
  var element = h2

  var results = [((String, String), URL)]()
  var category: String = ""

  outerLoop: while true {
    guard let sibling = try element.nextElementSibling() else { break }
    switch sibling.tagName() {
    case "h2":
      // We know the first h2 comes after the end of the categories.
      break outerLoop
    case "h3":
      category = clean(name: try sibling.text())
    case "ul":
      for child in sibling.children() {
        let plantName = clean(name: try child.text())
        let a = try child.getElementsByTag("a").first()!
        let href = try a.attr("href")
        let infoURL = URL(string:href, relativeTo:url)!
        results.append(((category, plantName), infoURL))
      }
    default:
      break
    }
    element = sibling
  }
  return results
}

func reduceHouseplantInfos(infos:[(HouseplantKey, HouseplantInfo)]) -> HouseplantCategoryDictionary {
  var result = HouseplantCategoryDictionary()
  for (key,info) in infos {
    if result[key.category] == nil {
      result[key.category] = HouseplantInfoDictionary()
    }
    result[key.category]![key.name] = info
  }
  return result
}

func scrapeHouseplants(url: URL) throws -> HouseplantCategoryDictionary {
  try reduceHouseplantInfos(
    infos: scrapeListOfHouseplants(url: url, html: try String(contentsOf: url))
      .map { ticket in
        (ticket.key, try scrapeHouseplantInfo(url: ticket.url,
                                              html:try String(contentsOf: ticket.url)))
      }
  )
}
```

This new code runs in the same amount of time as before. To make it run faster, we need to change
the scrapeHouseplants implementation to use `concurrentMap` instead of `map`:

```swift
// ThreadSafe and concurrentMap code from https://talk.objc.io/episodes/S01E90-concurrent-map

final class ThreadSafe<A> {
  private var _value: A
  private let queue = DispatchQueue(label: "ThreadSafe")
  init(_ value: A) {
    self._value = value
  }

  var value: A {
    return queue.sync { _value }
  }
  func atomically(_ transform: (inout A) -> ()) {
    queue.sync {
      transform(&self._value)
    }
  }
}

extension Array {
  func concurrentMap<B>(_ transform: (Element) throws -> B) rethrows -> [B] {
    let result = ThreadSafe(Array<B?>(repeating: nil, count: count))
    DispatchQueue.concurrentPerform(iterations: count) { idx in
      let element = self[idx]
      // TODO: Handle errors thrown here.
      let transformed = try! transform(element)
      result.atomically {
        $0[idx] = transformed
      }
    }
    return result.value.map { $0! }
  }
}

func scrapeHouseplants(url: URL) throws -> HouseplantCategoryDictionary {
  try reduceHouseplantInfos(
    infos: scrapeListOfHouseplants(url: url, html: try String(contentsOf: url))
      .concurrentMap { ticket in
        (ticket.key, try scrapeHouseplantInfo(url: ticket.url,
                                              html:try String(contentsOf: ticket.url)))
      }
  )
}
```

That improves our time to 1.06 seconds.

Instruments shows that we are using up to 6 threads. That's puzzling because my iMac Pro has 36
hardware threads. I believe parallelism is being limited to 6 threads due to
`String(contentsOf: url)` being limited to fetching no more than 6 simultaneous requests from a
single host.

We can refactor `scrapeHouseplants` to use URLSession directly:

```swift
func fetch(url: URL)-> String {
  var result: String?
  let semaphore = DispatchSemaphore(value: 0)
  DispatchQueue.global(qos: .userInitiated).async {
    let session = URLSession.shared
    let task = session.dataTask(with: url){data, response, error in
      result = String(decoding: data!, as: UTF8.self)
      semaphore.signal()
    }
    task.resume()
  }
  semaphore.wait()
  return result!
}

func scrapeHouseplants(url: URL) throws -> HouseplantCategoryDictionary {
  return try reduceHouseplantInfos(
    infos: scrapeListOfHouseplants(url: url, html: fetch(url: url))
      .concurrentMap { ticket in
        (ticket.key, try scrapeHouseplantInfo(url: ticket.url,
                                              html:fetch(url: ticket.url)))
      }
  )
}
```

This reduces the time to 0.58 seconds.

Instruments shows that that CPU utilization peaks at 28 threads, which is close to the 36 threads
that are available on my iMac Pro.

The final code looks like this:

```swift
import Foundation
import SwiftSoup

struct HouseplantInfo : Codable {
  var description: String
}

// The key is a houseplant name.
typealias HouseplantInfoDictionary = [String:HouseplantInfo]

// The key is a category name.
typealias HouseplantCategoryDictionary = [String:HouseplantInfoDictionary]

func scrapeHouseplantInfo(url: URL, html: String) throws -> HouseplantInfo {
  let document = try SwiftSoup.parse(html)

  var element : Element?

  let span = try document.select("#Description").first()
    ?? document.select("#Description_and_biology").first()
    ?? document.select("#Name_and_description").first()
    ?? document.select("#Plant_care").first()

  if span != nil {
    let h2 = span!.parent()!
    element = try h2.nextElementSibling()
  } else {
    // Start collecting text from the beginning of the web page.
    let div = try document.select(".mw-parser-output")
    element = div.first()?.children()[3]
  }

  var description = ""
  while element != nil {
    if element!.tagName().starts(with: "h") {
      break
    }
    description += try element!.text()
    element = try element!.nextElementSibling()
  }
  return HouseplantInfo(description: description)
}

// Remove […] or (…) text from a string.
func clean(name: String)-> String {
  name.replacingOccurrences(of: #"(\s*\[.*]|\s*\(.*\))"#, with: "", options: .regularExpression)
}

typealias HouseplantKey = (category:String, name: String)
typealias HouseplantTicket = (key: HouseplantKey, url:URL)

func scrapeListOfHouseplants(url: URL, html:String) throws -> [HouseplantTicket] {
  let document = try SwiftSoup.parse(html)

  let span = try document.select("#List_of_common_houseplants").first()!
  let h2 = span.parent()!
  var element = h2

  var results = [((String, String), URL)]()
  var category: String = ""

  outerLoop: while true {
    guard let sibling = try element.nextElementSibling() else { break }
    switch sibling.tagName() {
    case "h2":
      // We know the first h2 comes after the end of the categories.
      break outerLoop
    case "h3":
      category = clean(name: try sibling.text())
    case "ul":
      for child in sibling.children() {
        let plantName = clean(name: try child.text())
        let a = try child.getElementsByTag("a").first()!
        let href = try a.attr("href")
        let infoURL = URL(string:href, relativeTo:url)!
        results.append(((category, plantName), infoURL))
      }
    default:
      break
    }
    element = sibling
  }
  return results
}

func reduceHouseplantInfos(infos:[(HouseplantKey, HouseplantInfo)]) -> HouseplantCategoryDictionary {
  var result = HouseplantCategoryDictionary()
  for (key,info) in infos {
    if result[key.category] == nil {
      result[key.category] = HouseplantInfoDictionary()
    }
    result[key.category]![key.name] = info
  }
  return result
}

// ThreadSafe and concurrentMap code from https://talk.objc.io/episodes/S01E90-concurrent-map

final class ThreadSafe<A> {
  private var _value: A
  private let queue = DispatchQueue(label: "ThreadSafe")
  init(_ value: A) {
    self._value = value
  }

  var value: A {
    return queue.sync { _value }
  }
  func atomically(_ transform: (inout A) -> ()) {
    queue.sync {
      transform(&self._value)
    }
  }
}

extension Array {
  func concurrentMap<B>(_ transform: (Element) throws -> B) rethrows -> [B] {
    let result = ThreadSafe(Array<B?>(repeating: nil, count: count))
    DispatchQueue.concurrentPerform(iterations: count) { idx in
      let element = self[idx]
      // TODO: Handle errors thrown here.
      let transformed = try! transform(element)
      result.atomically {
        $0[idx] = transformed
      }
    }
    return result.value.map { $0! }
  }
}

func fetch(url: URL)-> String {
  var result: String?
  let semaphore = DispatchSemaphore(value: 0)
  DispatchQueue.global(qos: .userInitiated).async {
    let session = URLSession.shared
    let task = session.dataTask(with: url){data, response, error in
      result = String(decoding: data!, as: UTF8.self)
      semaphore.signal()
    }
    task.resume()
  }
  semaphore.wait()
  return result!
}

func scrapeHouseplants(url: URL) throws -> HouseplantCategoryDictionary {
  return try reduceHouseplantInfos(
    infos: scrapeListOfHouseplants(url: url, html: fetch(url: url))
      .concurrentMap { ticket in
        (ticket.key, try scrapeHouseplantInfo(url: ticket.url,
                                              html:fetch(url: ticket.url)))
      }
  )
}

func time<Result>(name: String = "",
                     warmup: Bool = false,
                     repeatCount: Int = 1,
                     _ callback:  @autoclosure () throws-> Result)
rethrows-> Result {
  if warmup {
    _ = try callback()
  }

  let start = Date()
  var result: Result?
  for _ in 0..<repeatCount {
    result = try callback()
  }
  print("\(name): \(-start.timeIntervalSinceNow / TimeInterval(repeatCount))")
  return result!
}

let url = URL(string:"https://en.wikipedia.org/wiki/Houseplant")!
let houseplants = try time(scrapeHouseplants(url:url))

let encoder = JSONEncoder()
encoder.outputFormatting = .prettyPrinted
let json = try encoder.encode(houseplants)
let jsonString = String(decoding: json, as: UTF8.self)
let outputFile = URL(fileURLWithPath: "/Users/jackpal/Desktop/houseplants.json")
try jsonString.write(to: outputFile, atomically: true, encoding: String.Encoding.utf8)
```
