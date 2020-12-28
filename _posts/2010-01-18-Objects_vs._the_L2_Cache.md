---
date: 2010-01-18 06:18
tags: 3d_graphics
title: Objects vs. the L2 Cache
---

An excellent little performance optimization presentation that shows how
important memory layout is for today's processors:

[Pitfalls of Object Oriented Programming](http://research.scee.net/files/presentations/gcapaustralia09/Pitfalls_of_Object_Oriented_Programming_GCAP_09.pdf)

The beginning of the talk makes the observation that since C++ was started in
1979 the cost of accessing uncached main memory has ballooned from 1 cycle to
400 cycles.

The bulk of the presentation shows the optimization of a graphics hierarchy
library, where switching from a traditional OO design to a structure-of-arrays
design makes the code run 3 times faster. (Because of better cache access
patterns.)
