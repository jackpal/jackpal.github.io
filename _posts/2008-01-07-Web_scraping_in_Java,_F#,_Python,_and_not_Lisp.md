---
date: 2008-01-07 14:13
tags: Java,F#,Lisp,Python,Web scraping
title: Web scraping in Java, F#, Python, and not Lisp
---

Yesterday I wrote a web scraper. A web scraper is a program that crawls over a
set of web pages, following links and collecting data. Another name for this
kind of program is a "spider", because it "crawls" the web.

In the past I've
written scrapers in [Java](http://www.java.com/en/) and
[F#](http://research.microsoft.com/fsharp/fsharp.aspx), with good results. But
yesterday, when I wanted to write a new scraper, I though I'd try using a
dynamically-typed language instead.

What's a dynamically-typed language you
ask? Well, computer languages can generally be divided into two camps,
depending on whether they make you declare the type of data that can be stored
in a variable or not. Declaring the type up front can make the program run
faster, but it's more work for the developer. Java and F#, the languages I
previously used to write a web scraper, are statically typed languages,
although F# uses type inference so you don't actually have to declare types
very often -- the computer figures it out for you.

In order to scrape HTML you need three things:

1. a language
2. a library that fetches HTTP pages
3. a library that parses the HTML into a tree of HTML tags

Unless you're using Mono or Microsoft's Common Language Runtime, the language
you choose will restrict the libraries that you can use.

So, the first thing I
needed to do was choose a dynamic language. Since I just finished reading
"[Practical Common Lisp](http://www.gigamonkeys.com/book/)", an excellent
advanced tutorial on the Lisp language, I though I'd try using Lisp. But that
didn't work out very well at all. Lisp has neither a standard implementation
nor a set of standard libraries for downloading web pages and parsing HTML. I
did some Googling to try and find some combination of parts that would work
for me. Unfortunately, it seemed that every web page I visited recommended a
different combination of libraries, and none of the combinations I tried
worked for me. In the end I just gave up in frustration.

Then, I turned to
[Python](http://www.python.org/). I had not used Python much, but I knew it
had a reputation as an easy-to-use language with a lot of easy-to-use
libraries. And you know what? It really was easy! I did some web searches,
copied some example code, and voila, I had a working web spider in about an
hour. And the program was easy to write every step of the way. I used the
standard CPython implementation for the language, Python's built-in urllib2
library to fetch the web data, and the [Beautiful
Soup](http://www.crummy.com/software/BeautifulSoup/) library for parsing the
HTML.

How does the Python compare to Java and F# for web scraping?

Python Benefits:

* Very brief, easy to write code
* Libraries built in or easy to find
* Lots of web examples
* I didn't have to think: I just used for loops and subroutine calls.
* Very fast turn-around.
* Easy to create and iterate over lists of strings.

Python non-issues for this application:

* Didn't matter that the language was slow, because this task is totally I/O bound.
* Didn't matter that the IDE is poor, using print and developing interactively was fine

F# Benefits:

* Good IDE (Visual Studio)
* Both URL fetching and HTML parsing libraries built in to CLR

F# Non-issues:

* Small community, few examples. (A non-issue because there is [an example of how to write a multi-threaded web scraper](http://cs.hubfs.net/forums/thread/94.aspx)!)

F# Drawbacks:

* The CLR libraries for URL fetching and HTML parsing are more difficult to use than Python. It takes more steps to complete similar operations.
* Strong typing gets in the way of writing simple code.
* odd language syntax compared to Algol-derived languages.
* Hard-to-understand error messages from the compiler.
* Mixed functional/imperative programming is more complicated than just imperative programing.
* The language and library encourages you to use advanced concepts to do simple things. In my web scraper I wrote a lot of classes and had methods that took complicated curried functions as arguments. This made the code hard to debug. In retrospect perhaps I should have just used lists of strings, the same as I did in Python. Since F# supports lists of strings pretty well, maybe this is my problem rather than F#'s. ;-)

Java benefits:

* Good debugger
* Good libraries
* Multithreading

Java drawbacks:

* Very wordy language
* Very wordy libraries

Lisp drawbacks:

* No standard implementation
* No standard libraries

Looking to the future, I'd be interested in writing a web scraper in
IronPython, which has good IDE support, and in C# 3.0, which has some support
for type inference.

In any event, I'm left with a very favorable impression of
Python, and plan to look into it some more. In the past I was put off from it
because it was slow, but now I see how useful it is when speed doesn't matter.

[Note: When I first wrote this article I was under the impression that CPython
didn't support threads. I since discovered (by reading the Python in a
Nutshell book) that it does support threads. Once I knew this, I was able to
easily add multi-threading to the web scraper. CPython's threads are somewhat
limited: only one thread is allowed to run Python code at a time. But that's
fine for this application, where the multiple threads spend most of their time
blocked waiting for C-based network I/O ]
