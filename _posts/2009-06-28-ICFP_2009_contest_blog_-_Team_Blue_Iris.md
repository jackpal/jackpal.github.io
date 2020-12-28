---
date: 2009-06-28 18:41
tags: ICFP Python
title: ICFP 2009 contest blog - Team Blue Iris
---

I just threw in the towel on the
[ICFP 2009 Programming Contest](http://icfpcontest.org/).

The problem this year was a set of 4 sub-
problems related to orbital mechanics, plus a virtual machine spec. The
virtual machine was used to enable the problems to be specified exactly,
without worrying about differences in the order of math operations.

Implementing the virtual machine was easy and fun. Unfortunately, actually
solving the final sub-problem required learning too much math and physics. I
was able to solve problems 1 and 2, and make an entry for the lightning round.
And I brute-forced a solution for problem 3. But now, 56 hours into the
contest, I am giving up on problem 4.

I can see the general outline of how to
solve it, but it would take sharper tools than I have now. For example, I'd
like a way of solving Lambert's equations, but I'm having trouble deriving the
code on my own, and the best example I've found while searching the web is a
[30-year-old NASA Space Shuttle Fortran program](http://www.google.com/url?sa=t&source=web&ct=res&cd=1&url=http%3A%2F%2Fntrs.nasa.gov%2Farchive%2Fnasa%2Fcasi.ntrs.nasa.gov%2F19790079987_1979079987.pdf&ei=PjFHSqXdKoumMYCuibgC&usg=AFQjCNHuISp5Jo_Yt8zra20RmTRqlnlhHQ&sig2=rV_Rm4DYdppoeV-KyqObXA).
 Also, I'm pretty tired, and this is affecting my
judgment. I don't think it's worth going on at this point.

Some fun things I did during the contest:

* Learned [Orbital Mechanics](http://www.braeunig.us/space/orbmech.htm), the study of how bodies move in space.
* Learned what a [Hohmann Transfer Orbit](http://en.wikipedia.org/wiki/Hohmann_transfer_orbit) is and why to use it.
* Learned what Lambert's Theorem is, and how to apply it to missile guidance.
* Wrote a Python program that did efficient calculations by generating a C program, compiling it, and then running it and talking to it via a pipe.
* Read a ton of Wikipedia articles, Google Books books and old NASA tech reports on orbit planning and course corrections.
* Learned how old-school Q-system guided missiles work. Very clever use of ground-based computers to compute coefficient matrices that were fed into simple on-board analog computers for the duration of the flight.

Highlights of the contest:

* Hanging out on IRC with 20 other contestants, trying to get the simulator to work.
* Getting problems 1 and 2 to work.
* Giving up on problem 3, then thinking of a brute-force way of solving it while in the parking lot, about to drive home. (Too bad I wasn't further inspired to solve problem 4.)
* Seeing the pretty pictures of the satellite orbits for problem 4.

Low-lights of the contest:

* Wasting an hour or so due to bugs in the specification
* Wasting an hour writing a Tkinter alternative to turtle graphics, then not being able to get a Tkinter window to show up, then realizing that Tkinter graphics are so limited that there's no feature benefit over using the already-working turtle graphics.
* The buggy scoring in problem 1 encourages people to program-to-the-scorer rather than solve the stated problem. I could probably increase my position in the standings by 10 places by hacking an "optimal" solution to problem 1 that uses all of the available fuel. But it seems like a waste of time.
* Having to give up on problem 4.

My advice to (future) contest organizers:

* Consider avoiding problems that require deep domain expertise. There's only so much orbital mechanics and numerical methods one can learn in 72 hours.
* Do whatever it takes to ensure that your VM machine spec is correct. In this case, just asking someone to spend a couple of hours implementing the spec would have exposed the show-stopping problems that everyone encountered.

Advice to myself for future contests:

* Pace yourself on day two, to avoid burning out on day 3.
* Be sure you understand the scoring system. For example problem 4 had partial credit, so a solution for problem 3 might have worked on problem 4.
* Scheduling work and family life to enable a free weekend for the contest worked out very well.
* Do more planning, and keep an eye on how the work is progressing, to avoid spending too much time on unnecessary work. "What's the simplest thing that could possibly work", and "you aint going to need it" are both good mottoes for this kind of contest.
* Take a little time to refactor the code as you go along, to avoid slowing down due to barnacles. (Example, passing the problem number all over the place because it was one of the inputs to the simulation, rather than special-casing it and setting it once in the VM.)

## Analysis of programming languages I used in the contest

I used Python and C. I
actually completed the lightning round in pure Python.

Benefits of Python

* Rapid development due to no compilation time, clean sparse syntax, well designed libraries, plenty of documentation and help available on-line.
* Turtle graphics made it dead simple to display satellite orbits
* The "struct" package made it dead simple to import and export binary data.
* The "subprocess" package made it easy to start and control other programs.
* Python 3.1's exact printout of floating point values made it easier to tell what the math was doing. (Other projects ran into problems with almost-zero values printing as zero.)

Drawbacks of Python

* Slow. I had to switch the simulation to C to have it run fast enough for problems 3 and 4
* Global Interpreter Lock (GIL) - meant I couldn't use multiple calculation threads written in Python in one process. (And my machine's got 8 hardware threads. :-) )
* Lack of static type checking is frustrating when program run times are long: I had a half-hour period wasted debugging simple errors that only occurred after 2 minutes of simulation run-time that a static type checker would have caught immediately. To be fair, I could also have caught them with unit testing.

Benefits of C

* Very simple to write code.
* Runs really fast. :-)

## Why My VM's Cool

I wanted to explain how my VM implementation worked, because
I think it probably ran faster than most other VMs.

I wrote a Python-based VM
as a reference. Then I wrote a VM generator that would read a VM spec and
generate hard-coded C to implement that specific spec. I used a "comparer" VM
to compare the output of the two VMs to make sure that there were no bugs in
the generated C version.

The hard-coded C VM was really hard coded to each
problem. All the VM instruction interpretation overhead was removed. In
addition, because the VM didn't have any indirection, the "mem" array was
replaced by hundreds of individual local variables. This allowed the C
compiler to do a very good job of optimizing the code, because it could easily
tell there was no aliasing.

I included a simple interactive shell in each
generated hard-coded C program. The console let you set inputs, run "n"
simulation steps, and read the outputs. This made it easy for me to control
the simulation from Python. It also made it easy to hand-test the C
simulation.

One feature I meant to add, but ran out of time/energy for, was to
save and restore the state of the simulation. This would have been very
helpful in solving problem 4.

## How I solved the problems

Problem 1: Wrote
Python VM. Implemented Hohmann transfers as described in [a Wikipedia
article](http://en.wikipedia.org/wiki/Hohmann_transfer_orbit).

Problem 2:
Calculated the correct time to start the Hohmann transfer analytically. (I
read about how to do this in a textbook I found through Google books.) Added
simple brute-force docking code to match orbits exactly. No fancy "S" curves
for me. (And wasted about an hour wondering why I didn't score, because early
versions of the contest problem spec didn't say you had to turn off your
engine for 900 seconds. I finally figured this out by disassembling the VM to
see why the score wasn't being set properly.)

Problem 3: Used my fast VM to
compute a table of where the satellites would be over time, then wrote a set
of nested for loops that tried various Hohmann transfers at various times
looking for a solution. The precomputed tables meant I could just look up
where the target satellite would be for any time in the future, rather than
having to do complex elliptical math.

Problem 4: Only got as far as simulating
and visualizing this one (boy the orbits are pretty!) Too tired to continue. I
was planning on using a variation of the brute-force approach that solved
problem 3, with save-and-restore of the simulator state, because I would have
to recompute the table of locations for my rocket each time its orbit changed.

### Conclusions

Upon reflection, I think that this particular contest, especially
problems 3 and 4, is best suited to a C/C++ solution. This is due to the heavy
reliance on numerical methods to calculate the optimal trajectories.

I liked
that there were multiple versions of each problem. It made it easier to tell
if we were making progress, and also allowed whole-program-level
parallelization to make use of muticore machines to solve the problems in
parallel.

While I expect the ultimate contest winners will code in a mutable-
state static-type-checked compiled language like C/C++, I predict Haskell will
do fairly well in the contest, due to its speed and the ease with which it
handles math. However, the winners will probably have a good grasp of orbital
mechanics, and it seems that someone who knows the math is more likely to be
using C-like-languages.

Well that's it, now I'm looking forward to next year!

P.S. Here's a Wiki with other team writeups:

[FUN Team ICFP 2009 Writeup / Wiki](http://wiki.freaks-unidos.net/icfp/2009/)
