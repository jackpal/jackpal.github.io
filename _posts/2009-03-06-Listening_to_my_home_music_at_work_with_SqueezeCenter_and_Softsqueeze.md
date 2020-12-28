---
date: 2009-03-06 18:50
tags: Home_computing audio_streaming Linux
title: Listening to my home music at work with SqueezeCenter and Softsqueeze
---

For some time I've wanted to listen to my home music collection on my computer
at work. I tried a bunch of different approaches, and finally came up with one
that works pretty well:

* I run the free [SqueezeCenter](http://www.slimdevices.com/pi_features.html) program on my home machine to serve music.
* I run the free [Softsqueeze](http://softsqueeze.sourceforge.net/) program on my work machine to listen to the music.
* To keep the whole world from listening to my music, [I set up an SSH tunnel](http://softsqueeze.sourceforge.net/ssh.html).

The resulting system works pretty well. In case you're wondering, the
SqueezeCenter program's main use is to serve music to the Squeezebox brand of
internet radios. The ability to use it with a regular computer, without
purchasing a Squeezebox internet radio, is a nice gesture on the part of the
Logitec company that makes and sells Squeezebox internet radios.
