---
date: 2020-12-29
tags: swift tutorial web_scraping
title: Web Scraping with SwiftSoup
---

* TOC
{:toc}
# Introduction
In this tutorial we'll use the open-source
[Swift Soup](https://github.com/scinfu/SwiftSoup) library to scrape
open-source houseplant data
from the [Wikipedia houseplants](https://en.wikipedia.org/wiki/Houseplant) page.

Our program will scrape this URL: https://en.wikipedia.org/wiki/Houseplant

Our program will produce a JSON object containing the scraped data.
It will look something like this:

```json
{
    "Tropical and subtropical": {
        "Aglaonema" : {
            "description": "These are evergreen perennial herbs with stems…'
        }
        …
    }
    …
}
```
<!--more-->
# Reverse engineering the web pages

In order to scrape the data, we need to understand the structure of two types of web pages:

1. The main [Houseplant](https://en.wikipedia.org/wiki/Houseplant) page.
2. The individual [Houseplant genus](https://en.wikipedia.org/wiki/Aglaonema)
pages.

The process of studying a web page to understand how it is structured is called
"reverse engineering". You are reversing the process that was used to create the web page.

## The Houseplant page

1. Use the Chrome web browser to visit the [Houseplant](https://en.wikipedia.org/wiki/Houseplant) page.
2. Choose View > Developer > View Source
3. Start reading the document to figure out how it's structured.

The structure of web pages changes over time, so this information may be out of date by the
time you read this. But as of the time this tutorial was written, the structure of the
Houseplants page looked like this:

```html
…
<h2>
    <span class="mw-headline"
     id="List_of_common_houseplants">
        List of common houseplants
    </span>
     …
</h2>

<h3>
    <span class="mw-headline"
     id="Tropical_and_subtropical">
        Tropical and subtropical
    </span>
     …
</h3>
…
<ul>
    <li>
        …
        <a href="/wiki/Aglaonema"
         title="Aglaonema">Aglaonema</a>
        …
    </li>
</ul>
```

Given this structure, we can find the houseplant links on this page as follows:

1. Find the h2 (second level headline) tag with the id "List_of_common_houseplants"
2. Find the series of h3 (third level headling) tags to get the categories.
3. Each category will be followed by a ul (unordered list) tag.
   1. Each li (list item) tag in the ul contains an "a" (anchor) tag to a specific
   houseplant species web page.

## The houseplant info page

Analyzing a typical houseplant species web page shows us that its structure is

```html
…
<h2>
    <span class="mw-headline" id="Description">
        Description
    </span>
    …
</h2>
<p>
    These are …
</p>
<p>
    Plants of the genus are native to humid, shady …
</p>
…
<h2>
```

Given this structure, we can find the houseplant species description text by:

1. Search for a h2 tag with id="Description":
2. Collect the text of all the following p (Paragraph) tage, until the next h2 tag.

# Time to Code

The process of scraping can be broken into the following steps:

1. Scrape the top-level houseplant page and collect links to individual houseplant info pages.
2. Scrape each houseplant info page.
3. Save the extracted data.

## Create a project

For simplicity we'll build our web scraper in Xcode as a Mac OS Command Line Tool. Create
a project by:

1. Open Xcode
2. Choose File > New > Project…
3. Choose "macOS"
4. Choose "Command Line Tool"
5. Tap Next
6. Fill in the dialog box:
   1. Product name: WebScraper
   2. Team: None
   3. Organization Identifier: com.example
   4. Language: Swift
7. Tap Next
8. Use the file dialog to Create the project where you like. (Typically in ~/Developer/)

## Scraping a houseplant info page

Let's start by scraping a houseplant info page, because it's a simpler task.

Open the main.swift file, delete all the existing contents, and type the following:

```swift
import Foundation

let url = URL(string:"https://en.wikipedia.org/wiki/Aglaonema")!
let html = try String(contentsOf: url)
print(html.prefix(200))
```

Use Command-R to build and run the code.

If all goes well, you will see the following text in the debug console.

```html
<!DOCTYPE html>
<html class="client-nojs" lang="en" dir="ltr">
<head>
<meta charset="UTF-8"/>
<title>Aglaonema - Wikipedia</title>
<script>document.documentElement.className="client-js";RLCONF={"wgBre
Program ended with exit code: 0
```
This is the source of the web page in HTML. We could use normal Swift string processing
APIs (like
[NSRegularExperession](https://developer.apple.com/documentation/foundation/nsregularexpression))
to parse the HTML and extract the structured data. But there
is an easier way, which is to use the SwiftSoup library.

## Add the Swift Soup package to your project

1. In Xcode, choose File > Swift Packages > Add Package Dependency…
2. Fill in the dialog box:
   1. https://github.com/scinfu/SwiftSoup
3. Tap Next
4. Tap Next again
5. Tap Finish

## Use SwiftSoup to find the description

```swift
import Foundation
import SwiftSoup

let url = URL(string:"https://en.wikipedia.org/wiki/Aglaonema")!
let html = try String(contentsOf: url)
let document = try SwiftSoup.parse(html)

print(try document.title())
```
Run the program and you should see this output:

```
…
Aglaonema - Wikipedia
Program ended with exit code: 0
```

Now, we can use the SwiftSoup APIs to find the span with the "Description" id:

```swift
import Foundation
import SwiftSoup

let url = URL(string:"https://en.wikipedia.org/wiki/Aglaonema")!
let html = try String(contentsOf: url)
let document = try SwiftSoup.parse(html)

let description = try document.select("#Description").first()!
print(description)
```

Which produces:

```html
<span class="mw-headline" id="Description">Description</span>
```

Now, we can use SwiftSoup to collect the text from the paragraphs that follow the span that we found. Note that the structure of the document is:

```html
…
<h2>
    <span id="Description">…
<p>
<p>
<h2>…
```

From the intial span that we found, we first have to traverse up the tree to the span's
parent h2 element. From the h2 element we can traverse the siblings, stopping when we run out of
p elements.

```swift
import Foundation
import SwiftSoup

let url = URL(string:"https://en.wikipedia.org/wiki/Aglaonema")!
let html = try String(contentsOf: url)
let document = try SwiftSoup.parse(html)

let descriptionSpan = try document.select("#Description").first()!
let h2 = descriptionSpan.parent()!
var element = h2
while true {
  guard let sibling = try element.nextElementSibling() else { break }
  if sibling.tagName() != "p" {
    break
  }
  print(try sibling.text())
  element = sibling
}
```

Which produces the promising:

```
These are evergreen perennial herbs with stems growing erect or decumbent and creeping. Stems that grow along the ground may root at the nodes. There is generally a crown of wide leaf blades which in wild species are often variegated with silver and green coloration. The inflorescence bears unisexual flowers in a spadix, with a short zone of female flowers near the base and a wider zone of male flowers nearer the tip. The fruit is a fleshy berry that ripens red. The fruit is a thin layer covering one large seed.[2]
Plants of the genus are native to humid, shady tropical forest habitat.[3]
```

Let's wrap the Swift code into a function that collects the text output:

```swift
func scrapeHouseplantSpecies(url: URL) throws -> String {
  let html = try String(contentsOf: url)
  let document = try SwiftSoup.parse(html)

  let descriptionSpan = try document.select("#Description").first()!
  let h2 = descriptionSpan.parent()!
  var element = h2

  var result = ""
  while true {
    guard let sibling = try element.nextElementSibling() else { break }
    if sibling.tagName() != "p" {
      break
    }
    result += try sibling.text()
    element = sibling
  }
  return result
}

let url = URL(string:"https://en.wikipedia.org/wiki/Aglaonema")!
print(try scrapeHouseplantSpecies(url: url))
```

Getting rid of the "[2]" and "[3]" footnotes in the text output is left as an exercise for the reader.

# Scraping the top-level Houseplant page

Scraping the top-level houseplant page proceeds similarly:

```swift
import Foundation
import SwiftSoup

func scrapeHouseplant(url: URL)throws {
  let html = try String(contentsOf: url)
  let document = try SwiftSoup.parse(html)

  let span = try document.select(
      "#List_of_common_houseplants")
      .first()!
  let h2 = span.parent()!
  var element = h2

   while true {
     guard let sibling = try element.nextElementSibling()
     else { break }
     print(sibling.tagName())
     if sibling.tagName() == "h2" {
       break
     }
     element = sibling
   }

let url = URL(string:"https://en.wikipedia.org/wiki/Houseplant")!
try scrapeHouseplant(url:url)

```

This produces a list of the tags:
```
h3
…
ul
h3
…
ul
h3
…
ul
h3
…
ul
h3
ul
h2
```

Write code to collect the desired data from the h3 and ul tags:

```swift
import Foundation
import SwiftSoup

func scrapeHouseplant(url: URL)throws {
  let html = try String(contentsOf: url)
  let document = try SwiftSoup.parse(html)

  let span = try document.select("#List_of_common_houseplants").first()!
  let h2 = span.parent()!
  var element = h2

  while true {
    guard let sibling = try element.nextElementSibling() else { break }
    switch sibling.tagName() {
    case "h2":
      return
    case "h3":
      print(try sibling.children().first()!.text())
    case "ul":
      for child in sibling.children() {
        print("  ", try child.text())
      }
    default:
      break
    }
    element = sibling
  }
}

let url = URL(string:"https://en.wikipedia.org/wiki/Houseplant")!
try scrapeHouseplant(url:url)
```

Which produces:

```
Tropical and subtropical
   Aglaonema (Chinese evergreen)
   Alocasia
   Anthurium
   Aphelandra squarrosa (zebra plant)
   …
Succulents
   Aloe vera
   …
```

From there, we can find the URL for the child page, and scrape it too:

```swift
func scrapeHouseplant(url: URL)throws {
  let html = try String(contentsOf: url)
  let document = try SwiftSoup.parse(html)

  let span = try document.select("#List_of_common_houseplants").first()!
  let h2 = span.parent()!
  var element = h2

  while true {
    guard let sibling = try element.nextElementSibling() else { break }
    switch sibling.tagName() {
    case "h2":
      return
    case "h3":
      print(try sibling.children().first()!.text())
    case "ul":
      for child in sibling.children() {
        print("  ", try child.text())
        let a = try child.getElementsByTag("a").first()!
        let href = try a.attr("href")
        let speciesURL = URL(string:href, relativeTo:url)!
        print(try scrapeHouseplantSpecies(url: speciesURL))
      }
    default:
      break
    }
    element = sibling
  }
}
```

Which produces:

```
Tropical and subtropical
   Aglaonema (Chinese evergreen)
These are evergreen …
   Alocasia
The large cordate …
   Anthurium
Fatal error: Unexpectedly found nil while unwrapping an Optional value: file WebScraper/main.swift, line 8
```

Uh-oh, We got a fatal error. What went wrong?

Well, if we look at the structure of the
[Anthurium](https://en.wikipedia.org/wiki/Anthurium) page, we can see that
it doesn't have any span with a "Description" id. Instead it has a span with a
"Description_and_biology" id:

```html
<span class="mw-headline" id="Description_and_biology">
    Description and biology
</span>
```

This sort of discrepancy is common when scraping web data. We can work around it by
checking for both ids:

```swift
  let span = try document.select("#Description").first()
   ?? document.select("#Description_and_biology").first()
  let h2 = span!.parent()!
```

That works fine for [Anthurium](https://en.wikipedia.org/wiki/Anthurium), but if we continue on,
we run into [Aphelandra squarrosa](https://en.wikipedia.org/wiki/Aphelandra_squarrosa),
which doesn't have an Description heading at all.

For this plant it looks like there's a "Plant_care" id we can use instead. And so on.
There's no science to this. After working our way through the documents, trying different
ways of parsing the web pages, the code looks like this:

```swift
func scrapeHouseplantSpecies(url: URL) throws -> String {
  let html = try String(contentsOf: url)
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
    // Stop at the next "h" tag. (h2, h3, whatever.)
    if element!.tagName().starts(with: "h") {
      break
    }
    description += try element!.text()
    element = try element!.nextElementSibling()
  }
  return description
}
```

# Creating objects

As it stands, the program produces unstructured text output. Let's store it in a set of
nested swift dictionaries:

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

func scrapeHouseplantInfo(url: URL) throws -> HouseplantInfo {
  let html = try String(contentsOf: url)
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

func scrapeHouseplants(url: URL)throws -> HouseplantCategoryDictionary {
  let html = try String(contentsOf: url)
  let document = try SwiftSoup.parse(html)

  let span = try document.select("#List_of_common_houseplants").first()!
  let h2 = span.parent()!
  var element = h2

  var categories = HouseplantCategoryDictionary()
  var categoryName: String = ""

  while true {
    guard let sibling = try element.nextElementSibling() else { break }
    switch sibling.tagName() {
    case "h2":
      // We know the first h2 comes after the end of the categories.
      return categories
    case "h3":
      // If there's an existing category name, add it to the
      categoryName = clean(name: try sibling.text())
      categories[categoryName] = HouseplantInfoDictionary()
    case "ul":
      for child in sibling.children() {
        let plantName = clean(name: try child.text())
        let a = try child.getElementsByTag("a").first()!
        let href = try a.attr("href")
        let infoURL = URL(string:href, relativeTo:url)!
        categories[categoryName]![plantName] =
          (try scrapeHouseplantInfo(url: infoURL))
      }
    default:
      break
    }
    element = sibling
  }
  return categories
}

let url = URL(string:"https://en.wikipedia.org/wiki/Houseplant")!
let houseplants = try scrapeHouseplants(url:url)

let encoder = JSONEncoder()
encoder.outputFormatting = .prettyPrinted
let json = try encoder.encode(houseplants)
let jsonString = String(decoding: json, as: UTF8.self)
print(jsonString)

let outputFile = URL(fileURLWithPath: "/Users/jackpal/Desktop/houseplants.json")
try jsonString.write(to: outputFile, atomically: true, encoding: String.Encoding.utf8)
```

# Directions for future work

This code works, but it could be improved:

+ Our code fetches pages sequentially. Each page takes only a short time to download, but
over a hundred web pages are fetched. We can speed up our scraper by fetching web pages in
parallel, perhaps by using
[DispatchQueue.concurrentPerform](https://developer.apple.com/documentation/dispatch/dispatchqueue/2016088-concurrentperform).

+ Extract additional information, such as photos and/or plant care instructions.
+ Clean up the scraped text to remove any malformed or extraneous text.
