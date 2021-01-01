---
date: 2020-12-27
tags: ipad jekyll swift web
title: Blogging from an iPad
---

After a day's hacking, I am pleased that I can update my github.io-based
blog using an iPad. Here's how I did it:

1. I researched the topic, finding some good info on [Avery Vine's post](https://www.averyvine.com/blog/programming/2019/10/04/publishing-to-jekyll-from-ipad-with-shortcuts-and-working-copy).

2. I wrote a script,
[ConvertBlogFromPublishToJekyll.swift](https://gist.github.com/jackpal/efe1a15e4b37b52550eef30fac129c2a),
to convert my posts from the markdown flavor used by
[Publish](https://github.com/JohnSundell/Publish) to the markdown flavor used by
[Jekyll](https://github.com/jekyll/jekyll).

That's it, [there's no step three](https://m.youtube.com/watch?v=YHzM4avGrKI). Whenever
I want to post to my blog from my iPad, I just edit the sources in
[Working Copy](https://workingcopyapp.com). Because the blog is backed by a git project,
I can also edit the blog from any other device that supports a git client, including
a regular Mac or PC.

This works because Jekyll support is built into github.io web pages. Whenever a new
commit is made, Github's servers automatically run the Jekyll app to regenerate my blog.
 
Pro tip: The Working Copy text editor can be switched from “Programming” mode to
“Natural” mode. Natural mode provides spell checking.
