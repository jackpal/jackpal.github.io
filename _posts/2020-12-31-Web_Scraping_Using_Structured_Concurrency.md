---
date: 2020-12-31
tags: parallelism structured_concurrency swift tutorial web_scraping
title: Web Scraping Using Structured Concurrency
---
# Introduction
In this tutorial we'll modify our
[toy web scraper](https://jackpal.github.io/2020/12/30/Speeding_up_Web_Scraping.html) to use
the newly proposed Swift async/await and Structured Concurrency APIs.


<!--more-->
# Swift async/await and Structured Concurrency

The Swift language is in the process of adding support for concurrency. Two of the new features are
[async/await](https://github.com/apple/swift-evolution/blob/main/proposals/0296-async-await.md) and
[Structured Concurrency](https://github.com/DougGregor/swift-evolution/blob/structured-concurrency/proposals/nnnn-structured-concurrency.md).
We can use these features to somewhat simplify the implementation of our web scraper.

Hat tip to
[Eneko Alonso](https://www.enekoalonso.com/2020/12/06/getting-started-with-async-await-in-swift.html)
for documenting how to install and configure a version of the Swift compiler toolchain that
supports these experimental features. I followed the instructions in his blogpost, except
instead of installing the Dec. 5th snapshot, I installed the more recent Dec. 23rd snapshot.

Once my project was set up to use the development toolchain, I replaced the DispatchQueue code with
the async/await and Structured Concurrency versions:

```swift
extension Array {
  // TODO: When withoutActuallyEscaping() supports async closures, use withoutActuallyEscaping
  // on the group.add() closure, and then to remove the unnecessary @escaping decoration.
  func asyncMap<B>(_ transform: @escaping (Element) async throws -> B) async throws -> [B] {
    await try Task.withGroup(resultType: (Int, B).self) { group in
      for i in self.indices {
        await group.add {
          (i, await try transform(self[i]))
        }
      }
      var result = [B?](repeating: nil, count: count)
      while let (index, transformed) = await try group.next() {
        result[index] = transformed
      }
      return result.map { $0! }
    }
  }
}

func fetch(url: URL)async -> String {
  await withUnsafeContinuation { continuation in
    let session = URLSession.shared
    let task = session.dataTask(with: url) { data, response, error in
      continuation.resume(returning: String(decoding: data!, as: UTF8.self))
    }
    task.resume()
  }
}

func scrapeHouseplants(url: URL) async throws -> HouseplantCategoryDictionary {
  return try reduceHouseplantInfos(
    infos: await scrapeListOfHouseplants(url: url, html: fetch(url: url))
      .asyncMap { ticket in
        (ticket.key, try scrapeHouseplantInfo(url: ticket.url,
                                              html:await fetch(url: ticket.url)))
      }
  )
}

func scrapeHouseplantsSync(url: URL) throws -> HouseplantCategoryDictionary {
  var houseplants = HouseplantCategoryDictionary()
  runAsyncAndBlock {
    // This line generates a warning: "Local var 'houseplants' is unsafe to reference in code that
    // may execute concurrently.". I think that warning is incorrect in this situation,
    // because of the "AndBlock" part of runAsyncAndBlock.
    houseplants = await try! scrapeHouseplants(url:url)
  }
  return houseplants
}

let url = URL(string:"https://en.wikipedia.org/wiki/Houseplant")!
let houseplants = try time(scrapeHouseplantsSync(url:url))
```

It's less code than before, requires less specialized knowledge, and it runs at about the same
speed. I look forward to using async/await and Structured Concurrency more as the features evolve
and hopefully ship in future versions of Xcode.

Thanks to [Eneko Alonso](https://www.enekoalonso.com/2020/12/06/getting-started-with-async-await-in-swift.html) for showing that this experimentation is even possible, and to the members of the
[Swift Evolution Forum](https://forums.swift.org/t/pitch-2-structured-concurrency/43452/41)
for answering my questions on how to use Structured concurrency.
