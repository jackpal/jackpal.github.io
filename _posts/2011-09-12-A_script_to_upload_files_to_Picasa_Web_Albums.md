---
date: 2011-09-12 04:10
tags: Picasa Web Albums Google+ Python
title: A script to upload files to Picasa Web Albums
---

Today I backed up all my family pictures and videos to Picasa Web Albums.

For several months I have been thinking of ways to backup my pictures to
somewhere outside my house. I wanted something simple, scalable and
inexpensive.

When I read that

[Google+ users can store unlimited pictures sized <= 2048 x 2048 and videos <= 15 minutes long](http://picasa.google.com/support/bin/http://picasa.google.com/support/bin/answer.py?answer=1224181),
I decided to try using Google+ to back up my media.

Full disclosure: I work for Google, which probably predisposes me to like and
use Google technologies.

Unfortunately I had my pictures in so many folders that it wasn't very
convenient to use either [Picasa](http://picasa.google.com/) or
[Picasa Web Albums Uploader](http://picasa.google.com/mac_tools.html) to upload them.

Luckily, I'm a programmer, and Picasa Web Albums has a public API for
uploading images. Over the course of an afternoon, I wrote a Python script to
upload my pictures and videos from my home computer to my Picasa Web Album
account. I put it up on GitHub:
[picasawebuploader](https://github.com/jackpal/picasawebuploader)

Good things:

* The [Google Data Protocol](http://code.google.com/apis/gdata/) is easy to use.
* Python's built-in libraries made file and directory traversal easy.
* OSX's built-in "sips" image processing utility made it easy to scale images.

Bad things:

* The documentation for the Google Data Protocol is not well organized or comprehensive.
* It's undocumented how to upload videos. Luckily I found a [Flicker-to-Picasa-Web](http://nathanvangheem.com/news/moving-to-picasa-update) script that showed me how.

To do:

* Use multiple threads to upload images in parallel.
* Prompt for password if not supplied on command line.
