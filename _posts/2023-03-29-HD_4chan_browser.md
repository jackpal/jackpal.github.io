---
date: 2023-03-29 13:48:57.256754
description: HD 4chan browser announcement.
tags: 4chan,SwiftUI,Xcode,iOS,iPhone,iPad
title: The HD 4chan Browser
---

I wrote [HD](https://github.com/jackpal/HD), a small SwiftUI app to browse the
4chan image board on an iPhone or iPad.

I'm proud of how nice the app is to use, and how fast it displays images, animations and videos.

<!--more-->

Apple doesn't allow 4chan apps in the App Store, so for now, the only way to use HD is to
build it yourself.

## Implementation details

The app source is small: only about 1000 lines of code. It makes extensive use of
SwiftUI and open source Swift Packages.

The app requires Xcode 14 and iOS 16.0 / iPadOS 16.0.

I used [Draw Things](https://apps.apple.com/us/app/draw-things-ai-generation/id6444050820) to create the app
icon. Pretty good for "programmer art".

Swift Packages are still a little rough to use. For example, I had to fork
the vlckit-spm package just to change
its version number to something compatible with Xcode:

| Name                                                         | Description                                            |
| ------------------------------------------------------------ | ------------------------------------------------------ |
| [FourChan api](https://github.com/jackpal/FourChanAPI)       | 4Chan content API.                                     |
| [HTMLString](https://github.com/jackpal/HTMLString)          | Convert HTML content to AttributedString and/or String |
| [Introspect](https://github.com/siteline/SwiftUI-Introspect) | Access the UIKit views that implement SwiftUI views.   |
| [Nuke](https://github.com/kean/Nuke)                         | Fast asynchronous image loader.                        |
| [SwiftSoup](https://github.com/scinfu/SwiftSoup)             | HTML parser.                                           |
| [SwiftyGIF](https://github.com/kirualex/SwiftyGif)           | GIF image loader.                                      |
| [vlckit-spm](https://github.com/tylerjonesio/vlckit-spm)     | VLC webm player.                                       |

## Disclaimer

[4chan.org](https://4chan.org) is an image board for fans of Japanese culture.

For historical reasons, 4chan allows a much wider range of content than most people are
comfortable with. I do not condone any of the posts or actions of any users who post on
4chan.

The HD App is not affiliated with or approved by the 4chan.org web site.
