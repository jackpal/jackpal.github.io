---
date: 2007-12-29 17:28
tags: Wayback Machine  souce code repos
title: Searching for old web pages:\ The Wayback machine is cool!
---

I was searching for information on LINJ, a Lisp language that compiles into
human-readable Java code. Unfortunately, the LINJ web site
http://www.evaluator.pt/linj.html, is currently offline.

Luckily, it turned
out that the Internet Archive [Wayback
machine](http://www.archive.org/web/web.php) had cached both that page, and
the download files that that page had pointed to. Very cool!

Similarly, I was
looking for the source to the Windows CE port of Quake 3, and found that the
project's web site http://www.noctemware.com/q3ce.html had been abandoned and
taken over by spammers. Luckily the Wayback machine had cached both the
original web page and the downloads.

Let this be a lesson to you aspiring open
source developers out there: It's better to store small open-source projects
in a large "won't-ever-go-away" source repository like
[SourceForge](http://www.sourceforge.net/) or [Google
Code](http://code.google.com/) than to use your own vanity domain hosting. Of
course, even using a large popular repository is not failure-proof. Some large
code repositories from the early days of the Internet, like DEC's ftp site,
have gone away after their owning company was bought by another company.
Perhaps some sort of distributed system of discoverable git repositories is
the answer.

In the case of Quake 3 for Windows CE, I've contacted the author,
Christien Rioux, and with his kind permission I've set up a code.google.com
project so that other people can more easily find the sources (and binaries):
<http://code.google.com/p/q3ce> .
