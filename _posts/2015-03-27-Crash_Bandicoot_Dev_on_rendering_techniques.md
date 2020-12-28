---
date: 2015-03-27 18:48
tags: Playstation 3D Crash Bandicoot
title: Crash Bandicoot Dev on rendering techniques
---

As seen on Hacker News:

[Hacker News Comment by dmbaggett](https://news.ycombinator.com/item?id=9277704)

Maybe Andy forgot to mention it; it's been a while since I've read the whole
series.

The code was C and lisp so it didn't really require any effort to port other
than replacing the rendering pipeline. And we used the SGIs to pre-render
every frame anyway, to precompute the polygon sort order. (The PS1 had no
Z-buffer, so you were stuck sorting polygons at run-time if you didn't do
something clever.)

So we already had the rendering pipeline ported. Obviously you couldn't save
your game to the memory card, etc. -- some stuff didn't work. But the game was
playable (albeit very frustrating with keyboard controls).

Some day I will write this up for real, but without going into detail, here's
a summary.

The camera in Crash was on a rail. It could rotate left, right, up, and down
(in Crash 2 and beyond, at least), but could not translate except by moving
forward/backward on the rail. This motivates a key insight: if you're only
rotating the camera, the sort order of the polygons in the scene cannot
change.

This allowed us to sample points on the rail and render the frame at each
sample point ahead of time, as a batch job, on the SGI using a Z-buffer. (We
may have done the Z-buffer with software; I don't remember.) Then we could
recover the polygon order of each frame by looking at the Z-buffer. And, even
better, at run-time we could simply  _not render at all_  those polygons that
weren't ultimately visible in the pre-rendered scene. This solved both the
sorting and clipping problem nicely, and made the look of the game closer to
3K polygons/frame vs. the 1K polygons we were actually rendering in real time.
(Many polygons were occluded by other polygons.)

The trick, though, was what exactly to do with this sort/occlusion
information. In a nutshell, what I did was write a custom delta-compression
algorithm tailored to the purpose of maintaining the sorted polygon list from
frame to frame, in R3000 assembly language. Miraculously, this ended up being
quite feasible because the delta between frames was in practice very small --
a hundred bytes or so was typical. And if a transition was too heavyweight
(i.e., the delta was too big) we'd either sample more finely in that area or
tell the artists to take stuff out. :)

One thing nobody talks about but which is obvious in retrospect is that
without a Z-buffer you're pretty screwed: sorting polygons is  _not_  O(N lg
N) -- it's O(N^2). This is because polygons don't obey the transitivity
property, because you can have cyclic overlap. (I.e., A > B and B > C does
_not_  imply A > C). This is why virtually every game from that era has
flickery polygons -- they were using bucket sorting, which has the advantage
of being linear time complexity, but the disadvantage of being wrong, and
producing this flickery effect as polygons jump from bucket to bucket between
frames.

I'll leave the matter of weaving the foreground characters -- Crash himself
and the other creatures -- into the pre-sorted background for another day.
