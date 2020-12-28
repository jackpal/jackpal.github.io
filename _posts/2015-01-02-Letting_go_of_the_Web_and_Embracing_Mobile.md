---
date: 2015-01-02 22:27
tags: strategy,predictions,mobile,iOS,Android
title: Letting go of the Web and Embracing Mobile
---

When I started working on Android in 2007, I had never owned a mobile phone.
When Andy Rubin heard this, he looked at me, grinned, and said "man, you're on
the wrong project!"

But actually, being late to mobile worked out well. In the early days of
Android the daily build was rough. Our [Sooner and
G1](http://en.wikipedia.org/wiki/HTC_Dream) prototypes often wouldn't work
reliably as phones, and that drove the other Android developers crazy. But
since I was not yet relying on a mobile phone, it didn't bother me much.

Seven years later, [mobile's eaten the world](http://ben-
evans.com/benedictevans/2014/10/28/presentation-mobile-is-eating-the-world).
But I still haven't internalized what that means. I think I'm still too
personal-computer-centric in my thinking and my planning.

Here's some recent changes that I'm still trying to come to grips with:

* Android and iOS are the important client operating systems. The web is now a legacy system.
* Containerized Linux is the important server operating system. Everything else is legacy.
* OS X is the important programmer's desktop OS (because it's required for iOS development, and adequate for Android and containerized Linux development.)
* The phone is the most important form factor, with tablet in second place.
* Media has moved from local storage to streaming.
* Programming cultural discussion has moved from blogs & mailing lists to Hacker News, Reddit & Twitter. (To be fair, these new forums mostly link back to blog posts for the actual content.)

In reaction, I've stopped working on the following projects:

* [Terminal Emulator for Android](https://play.google.com/store/apps/details?id=jackpal.androidterm&hl=en). When I started this project, all Android devices had hardware keyboards. But those days are long gone. And unfortunately for most people there isn't a compelling use case for an on-the-device terminal emulator. The compelling command-line use cases for mobile are SSH-ing from the mobile device to another machine, and adb-ing into the Android device from a desktop.
* [BitTorrent clients](https://github.com/jackpal/Taipei-Torrent). My clients were written just for fun, to learn how to use the Golang and node.js networking libraries. With the fun/learning task accomplished, and with BitTorrent usage in decline, there isn't much point in working on these clients. (Plus I didn't like dealing with bug reports related to sketchy torrent sites.)
* New languages. For the platforms I'm interested in, the practical languages are C/C++, Java, Objective C, and Swift. (And Golang for server-side work.)
  * I spent much of the past seven years experimenting with dynamic languages, but a year of using Python and JavaScript in production was discouraging. The brevity was great, but the loss of control was not.

Personal Projects for 2015

First, I'm going to port my ancient game Dandy to mobile. It needs a lot of
work to "work" on mobile, but it's a simple enough game that the port should
be possible to do on a hobby time budget. I'm probably going to go closed-
source on this project, but I may blog the progress, because the process of
writing down my thoughts should be helpful.

After that, we'll see how it goes!
