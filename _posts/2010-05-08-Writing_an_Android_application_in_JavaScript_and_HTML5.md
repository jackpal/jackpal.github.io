---
date: 2010-05-08
tags: android javascript phonegap games
title: Writing an Android application in JavaScript and HTML5
---

![Screenshot of word game](/assets/posts/2010-05-08-Writing_an_Android_application_in_JavaScript_and_HTML5-wordgame.jpg)
Happy Mother's Day everyone! This year I wrote an Android app for my wife for
Mother's Day. How geeky is that?

The reason I wrote it is that my wife's favorite Android application, Word Mix
Lite, recently added annoying banner ads. While the sensible thing to do might
have been to switch to another app, or perhaps upgrade to the paid version of
Word Mix, I thought it might be interesting to see if I could write my own
replacement.

And as long as I was writing the app, I though I'd try writing it using
JavaScript. Normally Android applications are written in Java, but it's also
possible to write them in JavaScript, by using a shell application such as
[PhoneGap](http://phonegap.pbworks.com/Getting-Started-with-PhoneGap-\(Android\)).

Why use JavaScript instead of Java? Well, hack value mostly. On the plus side
it's a fun to write! Another potential plus is that it is theoretically
possible to run the same PhoneGap application on both Android and Apple.

The drawback is that PhoneGap is not very popular yet, especially among
Android developers, and so there is relatively little documentation. I ran
into problems related to touch events and click events that I still haven't
completely solved.

It took about three days to write the application. The first day was spent:

* Finding [a public domain word list](http://wordlist.sourceforge.net/12dicts-readme.html).
* Filtering the word list to just 3-to-6 character words.
* Build a "trie" to make it fast to look up anagrams.
* Output the trie as a JSON data structure.

The second day was spent writing the core of the word jumble game.

* This was all done on a desktop, using Chrome's built-in Javascript IDE.

The third day was spent:

* Porting to PhoneGap.
* Debugging Android-specific issues.
* Tuning the gameplay for the phone.

The project went pretty well.

* JavaScript is a very easy language to work in.
* CSS styling was fairly easy to learn.
* Targeting a single browser made it relatively easy to use the DOM.
* The Chrome JavaScript debugger was very helpful.
* The web has a wealth of information on JavaScript and HTML / CSS programming information.

In the end there were unfortunately a few button-event UI glitches I couldn't
solve in time for Mother's Day. When I get some spare time I'd like to revisit
this project and see if I could use the [JQTouch](http://www.jqtouch.com/)
library to solve my UI glitches. I'd also like to make an open source release
of a reskinned (non-Mother's Day themed) version of this app. And then who
knows? Maybe I'll release it for the other phones that PhoneGap supports as
well. :-)
