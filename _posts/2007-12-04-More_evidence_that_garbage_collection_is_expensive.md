
---
date: 2007-12-04 17:58
tags: gc
title: More evidence that garbage collection is expensive
---

As seen on [Lambda the Ultimate.org](http://lambda-the-ultimate.org/node/2552)

> Quantifying the Performance of Garbage Collection vs. Explicit Memory
> Management We compare explicit memory management to both copying and non-
> copying garbage collectors across a range of benchmarks, and include real
> (non-simulated) runs that validate our results. These results quantify the
> time-space tradeoff of garbage collection: with five times as much memory, an
> Appel-style generational garbage collector with a non-copying mature space
> matches the performance of explicit memory management. With only three times
> as much memory, it runs on average 17% slower than explicit memory management.
> However, with only twice as much memory, garbage collection degrades
> performance by nearly 70%. When physical memory is scarce, paging causes
> garbage collection to run an order of magnitude slower than explicit memory
> management.
