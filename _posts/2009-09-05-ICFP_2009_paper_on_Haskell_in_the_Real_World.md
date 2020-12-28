---
date: 2009-09-05 17:05
tags: Haskell, Erlang, Ruby
---

# ICFP 2009 paper on Haskell in the Real World

Here's a good paper on using Haskell to write a commercial application. The
authors are practical commercial programmers who tried Haskell to see if it
was a more effective language than Ruby:
[Experience Report: Haskell in the “Real World” Writing a Commercial Application in a Lazy Functional Language](http://portal.acm.org/ft_gateway.cfm?id=1596578&type=pdf&coll=portal&dl=ACM&CFID=505049525&CFTOKEN=505049525)
Of special interest is the "Problems and Disadvantages" section. It seems that
space leaks which are a continuing source of trouble in the authors'
application. Reading this paper reminds me of
[Tenerife Skunkworks Haskell vs Erlang Reloaded](http://web.archive.org/web/20070701221306/wagerlabs.com/2006/01/01/haskell-vs-erlang-reloaded).
In that experiment a developer found that Erlang
was much better than Haskell for real-time programming.
