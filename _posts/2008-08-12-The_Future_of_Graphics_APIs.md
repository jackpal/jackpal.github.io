
---
date: 2008-08-12 14:48
tags: 3d,OpenGL,game consoles,predictions
title: The Future of Graphics APIs
---

The OpenGL 3.0 spec was released this week, just in time for SigGraph. It
turns out to be a fairly minor update to OpenGL, little more than a
codification of existing vendor extensions. While this disappoints OpenGL
fans, it's probably the right thing to do. Standards tend to be best when they
codify existing practice, rather than whey they try to invent new ideas.

What about the future?

The fundamental forces are:

+ GPUs and CPUs are going to be on the same die
+ GPUs are becoming general purpose CPUs.
+ CPUs are going massively multicore

Once a GPU is a general purpose CPU, there's little reason
to provide a standard all-encompasing rendering API. It's simpler and easier
to give an OS and a C compiler, and a reference rendering pipeline. Then let
the application writer customize the pipeline for their application.

The big unknown is whether any of the next-generation video game consoles
will adopt
the CPU-based-graphics approach. CPU-based graphics may not be cost
competitive soon enough for the next generation of game consoles.

Sony's a
likely candidate - it's a natural extension to the current Cell-based PS3.
Microsoft would be very comfortable with a Larrabee-based solution, given
their OS expertiese and their long and profitable relationship with Intel.
Nintendo's pretty unlikely, as they have made an unbelievable amount of money
betting on low-end graphics. (But they'd switch to CPU-based graphics in an
instant if it provided cost savings. And for what it's worth, the N64 did have
DSP-based graphics.)
