
---
date: 2009-03-25 21:37
tags: Larrabee
title: Intel describes Larrabee instruction set
---

Perhaps in preparation for Friday's GDC talks by Michael Abrash and Tom
Forsyth, Intel has described the Larrabee instruction set:

[Prototype Primitives Guide](http://software.intel.com/en-us/articles/prototype-primitives-guide/)

Intel includes a C source file that implements their new
instruction set, so people can play around with the instructions before
Larrabee ships.

The instruction set looks alot like a typical GPU shader
instruction set. Lots of "log" and "rsqrt" type instructions. But there are
also some interesting variations on MADD, such as MADD233_{PI,PS}, which I
assume help shave cycles off of inner loops. The compress and expand
instructions also look very useful. I look forward to reading code examples
from Abrash and Forsyth in the near future!
