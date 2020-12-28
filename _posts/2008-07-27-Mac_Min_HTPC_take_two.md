---
date: 2008-07-27 12:38
tags: htpc mac_mini
title: Mac Min HTPC take two
---

I just bought another Mac Mini to use as a HTPC (home theater PC). I tried
this a year ago, but was not happy with the results. But since then I've
become more comfortable with using OS X, so today I thought I'd try again.

Here's my quick setup notes:

* I'm using a Mac Mini 1.83 Core 2 Duo with 1 GB of RAM. This is the cheapest Mac Mini that Apple currently sells. I thought about getting an AppleTV, but I think the Mini is easier to modify, has more CPU power for advanced codecs, and can be used as a kid's computer in the future, if I don't like using it as an HTPC. I also have dreams of writing a game for the Mini that uses Wiimotes. I think this would be easier to do on a Mini than an AppleTV, even though the AppleTV has a better GPU.
* I'm using "Plex" as for viewing problem movies, and I think it may end up becoming my main movie viewing program. It's the OSX version of Xbox Media Center. (Which is a semi-legal program for a hacked original Xbox. The Plex version is legal because it doesn't use the unlicensed Xbox code.) The UI is a little rough. (Actually, by Mac standards it's _very_ rough. :-) ) Plex has very good codec support and lots of options for playing buggy or non-standard video files.
* I connected my Mac Mini to my media file server using gigabit ethernet. This made Front Row feel much snappier than when I was using an 802.11g wireless connection.
* I installed the [Perian](http://perian.org/) plugin adds support for many popular codecs to Quicktime and Front Row.
* I set up my Mac Mini to automatically mount my file server share at startup and when coming out of sleep. [Detailed instructions here.](http://forums.mactalk.com.au/13/42160-solution-how-auto-reconnect-network-drive-after-resuming-sleep.html) Synopsis: Create an AppleScript utility to mount the share, put the utility in your Login Items so that it's run automatically at startup, and finally use [SleepWatcher](http://www.bernhard-baehr.de/) to run the script after a sleep.
* I added FrontRow to my Login Items (Apple Menu:System Preferences...:Accounts:Login Items) to start Front Row at startup.
* I administer my Mini HTPC using VNC from a second computer. I don't have a keyboard or mouse hooked up to the HTPC normally. I disabled the Bluetooth keyboard detection dialog using Apple Menu:System Preferences...:Bluetooth:Advanced... then uncheck "Open Bluetooth Setup Assistant at startup when no input device present".

Things I'm still working on:

* No DVR-MS codec support in Perian, and therfore none in Front Row. I have to use my trusty Xbox 360 or VLC to view my Microsoft Windows Media Center recordings.
