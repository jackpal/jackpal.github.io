---
date: 2020-12-30
tags: parallelism swift tutorial web_scraping
title: Speeding up Web Scraping
---
# Introduction
In this tutorial we'll use Swift concurrency primitives to speed up our toy web scraper.

Original scraping time: 18 seconds

Optimized scraping time: 0.6 seconds

<!--more-->
# Web Scraping is an Embarrassingly Parallel Problem

Our toy web scraping problem can be divided into three steps:

1) Fetch the master web page that contains a list of houseplants.
2) For each houseplant, fetch its web page.
3) Combine the information from the first 2 steps into a data structure, and write that to disk.

The second phase can be done in parallel.

## Measuring performance

Starting with the existing web scraper, let's add a simple performance benchmark:

```swift
func measure<Result>(name: String = "",
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

let houseplants = try measure(name:"Scrape", scrapeHouseplants(url:url))
```

When I measured the code, it took about 17.73 seconds to run
each iteration, with a variation of about 0.3 seconds between runs.

## Parallelising code in Swift

We need to split our existing scrapeHouseplants function into three functions:

1. scrapeListOfHousePlants: A function that scrapes the main houseplants page and produces a list
of individual houseplants.
2. reduceHouseplantInfos: A function that combines a list of HouseplantInfos into the final
HouseplantCategoryDictionary
3. scrapeHouseplants: An overall function that coordinates the whole scraping process.

While we're refactoring, we'll also change scrapeHouseplantInfo to pass in the html content, rather
than fetching it inside the scrapeHouseplantInfo function. This separation will come in handy later.

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

typealias HousePlantKey = (category:String, name: String)
typealias HousePlantTicket = (key: HousePlantKey, url:URL)

func scrapeListOfHousePlants(url: URL, html:String) throws -> [HousePlantTicket] {
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

func reduceHouseplantInfos(infos:[(HousePlantKey, HouseplantInfo)]) -> HouseplantCategoryDictionary {
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
    infos: scrapeListOfHousePlants(url: url, html: try String(contentsOf: url))
      .map { ticket in
        (ticket.key, try scrapeHouseplantInfo(url: ticket.url,
                                              html:try String(contentsOf: ticket.url)))
      }
  )
}
```

This new code runs in the same amount of time as before. To make it run faster, we need to change
the scrapeHouseplants implementation to use a `concurrentMap` instead of `map`:

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
    infos: scrapeListOfHousePlants(url: url, html: try String(contentsOf: url))
      .concurrentMap { ticket in
        (ticket.key, try scrapeHouseplantInfo(url: ticket.url,
                                              html:try String(contentsOf: ticket.url)))
      }
  )
}
```

That improves our time to 4.7 seconds. While it's a great improvement, it's
much less of an improvement than we expected. What's going on?

Instruments shows that we're CPU bound parsing the documents. By default Xcode builds projects using
non-optimized "Debug" mode.

Editing the configuration to specify a Release build brings the total time down to 1 second.

But we can do even better. We're fetching web pages using `URL(contents:)`, which runs
synchronously. This limits the number of simultaneous fetches to the number of active threads.
`DispatchQueue.concurrentPerform` limits the number of threads to match the number of hardware
threads. Some Macs have as few as 4 hardware threads.

Refactoring `scrapeHouseplants` to use NSURLSession to fetch URLs asynchronously reduces the total
time to 0.6 seconds.

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

typealias HousePlantKey = (category:String, name: String)
typealias HousePlantTicket = (key: HousePlantKey, url:URL)

func scrapeListOfHousePlants(url: URL, html:String) throws -> [HousePlantTicket] {
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

typealias HouseplantInfos = [(HousePlantKey, HouseplantInfo)]

func reduceHouseplantInfos(infos:HouseplantInfos) -> HouseplantCategoryDictionary {
  var result = HouseplantCategoryDictionary()
  for (key,info) in infos {
    if result[key.category] == nil {
      result[key.category] = HouseplantInfoDictionary()
    }
    result[key.category]![key.name] = info
  }
  return result
}

// Adapted from https://talk.objc.io/episodes/S01E90-concurrent-map
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
  
  // Adapted from https://talk.objc.io/episodes/S01E90-concurrent-map
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

  func asyncMap<B>(_ transform: @escaping (Element, @escaping (B)->Void)-> Void,
                   callback:@escaping ([B]) -> Void) {
    let result = ThreadSafe(Array<B?>(repeating: nil, count: count))
    DispatchQueue.global(qos: .userInitiated).async {
      let group = DispatchGroup()
      enumerated().forEach { idx, element in
        group.enter()
        transform(element) { transformed in
          result.atomically {
            $0[idx] = transformed
          }
          group.leave()
        }
      }
      group.wait()
      callback(result.value.map { $0! })
    }
  }

}

typealias FetchedHouseplantHTML = (ticket:HousePlantTicket, html:String)

func asyncScrapeHouseplants(url: URL, callback:@escaping (HouseplantCategoryDictionary)->Void) {
  let session = URLSession.shared
  let dataTask = session.dataTask(with: url) { data, response, error in
    let tickets = try! scrapeListOfHousePlants(url: url, html: String(decoding: data!, as: UTF8.self))
    tickets.asyncMap( {(ticket, callback: @escaping (FetchedHouseplantHTML)->Void) in
      let dataTask = session.dataTask(with: ticket.url){ data, response, error -> Void in
        callback(FetchedHouseplantHTML(ticket:ticket, html:String(decoding: data!, as: UTF8.self)))
      }
      dataTask.resume()
    }, callback: { fetchedDocs in
      let houseplantInfos = fetchedDocs.concurrentMap {fetchedDoc in
        (fetchedDoc.ticket.key, try! scrapeHouseplantInfo(url: fetchedDoc.ticket.url, html:fetchedDoc.1))
      }
      callback(reduceHouseplantInfos(infos:houseplantInfos))
    })
  }
  dataTask.resume()
}

func scrapeHouseplants(url: URL) -> HouseplantCategoryDictionary {
  var result: HouseplantCategoryDictionary?
  DispatchQueue.global(qos: .userInitiated).sync {
    let group = DispatchGroup()
    group.enter()
    asyncScrapeHouseplants(url: url) {
      result = $0
      group.leave()
    }
    group.wait()
  }
  return result!
}

/// A toy performance measuring wrapper.
func measure<Result>(name: String = "",
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
let houseplants = measure(scrapeHouseplants(url:url))

let encoder = JSONEncoder()
encoder.outputFormatting = .prettyPrinted
let json = try encoder.encode(houseplants)
let jsonString = String(decoding: json, as: UTF8.self)

let outputFile = URL(fileURLWithPath: "/Users/jackpal/Desktop/houseplants.json")
try jsonString.write(to: outputFile, atomically: true, encoding: String.Encoding.utf8)
```
