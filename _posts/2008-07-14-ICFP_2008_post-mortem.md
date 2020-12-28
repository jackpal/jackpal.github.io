---
date: 2008-07-14 13:07
tags: icfp contest Blue_Iris Python
title: ICFP 2008 post-mortem
---

This year's [ICFP](http://www.icfpcontest.org/)[
contest](http://www.icfpcontest.org/) was a traditional one: Write some code
that solves an optimization problem with finite resources, debug it using
sample data sets, send it in, and the judging team will run it on secret
(presumably more difficult) data sets, and see whose program does the best.
The problem was to create a control program for an idealized Martian rover
that had to drive to home base while avoiding craters, boulders, and moving
enemies.

I read the problem description at noon on Friday, but didn't have
time to work on the contest until Saturday morning.

The first task was to
choose a language. On the one hand, the strict time limit argued for an easy-
to-hack "batteries included" language like Python, for which libraries, IDEs,
and cross-platform runtime were all readily available. On the other hand, the
requirement for high performance and ability to correctly handle unknown
inputs argued for a type safe, compiled language like ML or O'Caml.

I spent a
half an hour trying to set up an O'Caml IDE under Eclipse, but unfortunately
was not able to figure out how to get the debuger to work. Then I switched to
Python and the [PyDev IDE](http://pydev.sourceforge.net/index.html), and never
ran into a problem that made me consider switching back.

I realize that the
resulting program is much slower than a compiled O'Caml would be, and it
probably has lurking bugs that the O'Caml type system would have found at
compile time. But it's the best I could do in the limited time available for
the contest.

It was very pleasant to develop in Python. It's got a very nice
syntax. I was never at a loss for how to proceed. Either it "just worked", or
else a quick web search would immediately find a good answer. (Thanks Google!)

The main drawback was that the Python compiler doesn't catch simple mistakes
like uninitialized variables until run time. Fortunately that wasn't too much
of a problem for this contest, as the compile-edit-debug cycle was only a few
seconds long, and it only took a few minutes to run a whole test suite.

The
initial development went smoothly: I wrote was the code to connect to the
simulation server and read simulation data from the server. Then I created
classes for the various types of objects in the world, plus a class to model
the world as a whole. I then wrote a method that examined the current state of
the world and decided what the Martian rover should do next. Finally I wrote a
method that compared the current and desired Martian rover control state, and
sent commands back to the simulation server to update the Martian rover
control state.

The meat of the problem is deciding how to move the rover. The
iterative development cycle helped a lot here -- by being able to run early
tests, I quickly discovered that the presence of fast-moving enemies put a
premium on high speed movement. You couldn't cautiously analyze the world and
proceed safely, you had to drive for the goal as quickly as possible.

My
initial approach was to search for the closest object in the path of the
rover, and steer around it. This worked, but had issues in complicated
environments. Then I switched to an idea from Craig Reynolds' [Not Bumping
Into Things](http://www.red3d.com/cwr/nobump/nobump.html) paper: I rendered
the known world into a 1D frame buffer, and examined the buffer to decide
which way to go. That worked well enough that I used it in my submission.

I spent about fourteen hours on the contest: Two hours reading the problem and
getting the IDE together, ten hours over two days programming and debugging,
and about two hours testing the program on the Knoppix environment and
figuring out how to package and submit the results.

## Things I wish I had had time to do

* My rover is tuned for the sample data sets. The organizers promised to use significantly different data sets in the real competition. Unfortunately, I didn't have time to adapt the program to these other data sets, beyond some trivial adjustments based on potential differences in top speed or sensor range.
* I model the world at discreet times, and don't account for the paths objects take over time. I can get away with this because I'm typically traveling directly towards or away from important obstacles, so their relative motion is low. But I would have trouble navigating through whirling rings of Martians.
* I don't take any advantage of knowledge of the world outside the current set of sensor data. The game explicitly allows you to remember the world state from run to run during a trial. This could be a big win for path planning when approaching the goal during the second or later trials.
* I don't do any sort of global path planning. A simple maze around the goal would completely flummox my rover.

I very much enjoyed the contest this year. I look forward to finding out how
well I did, as well as reading the winning programs. The contest results will
be announced at the actual [ICFP conference](http://www.icfpconference.org/)
in late September.
