---
date: 2009-09-08
tags: games javascript brick_break breakout
title: Brick Break - a Javascript Breakout clone
---

![Brick Break game screenshot](/assets/posts/2009-09-08-Brick_Break_-_a_Javascript_Breakout_clone-brickbreak.png)

This weekend I wrote a Javascript clone of the old Atari "Breakout" game.
Thanks to the "Canvas" tag it was very easy to write, but I did run into a few
problems:

+ Javascript math is always floating point, so I had to use the "Math.floor"
function to convert the results of a division to an integer. This was in the
brick collision detection logic, where I am converting the ball's (x,y)
coordinates to the bricks that the ball might be hitting.

+ I was evaluating document.getElementById too early in the document lifecycle,
before the corresponding elements existed. This took me a long time to
diagnose -- I ended up just moving the getElementById calls to their run-time
use, rather than trying to cache the results.

[Jack's Brick Break Breakout clone](/assets/posts/2009-09-08-Brick_Break_-_a_Javascript_Breakout_clone-brickbreak.html)
