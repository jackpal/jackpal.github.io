---
date: 2017-08-06 03:16
description: My son and I competed as team "Blue Iris" in the ICFP 2017 programming contest.
image: pizza.jpg
tags: Contests ICFP
title: Team Blue Iris ICFP 2017 Programming Contest Postmortem
---

My son and I competed as team "Blue Iris" in the [ICFP 2017 programming contest](https://icfpcontest2017.github.io/).

[The ICFP programming contest](https://en.wikipedia.org/wiki/ICFP_Programming_Contest) is an annual
3-day programming contest sponsored by the [International Conference on Functional Programming](http://www.icfpconference.org/).
[Functional programming](https://en.wikipedia.org/wiki/Functional_programming) is an
approach to writing programs that stresses writing as much of the program as
possible in terms of functions. That's as opposed to the more commonly used
[imperative programming](https://en.wikipedia.org/wiki/Imperative_programming).

In the contest, people form teams to compete for three days to solve a
problem, using any combination of programming languages. People compete for
the joy of problem solving in the language of their choice. It's common for
people to use outlandish or obscure programming languages. It's sort of like
the [Wacky Races](https://en.wikipedia.org/wiki/Wacky_Races) of programming
contests.

[![Wacky Racers](/assets/posts/2017-08-06-Team_Blue_Iris_ICFP_2017_Programming_Contest_Postmortem-wackyracers.jpg)

I've competed in this contest about six times over the past 10 years. This
year was the first year my son joined me. My son's got about a year's worth of
programming experience, mostly in Java. He was eager to compete in the contest
with me this year.

This year we happened to be on vacation in Taiwan during the contest. It's hot
and humid in Taiwan, but the internet infrastructure is great, so we expected
to not have any problem competing.

The contest started at 8pm Friday night Taiwan time. I prepared by buying a 2
liter bottle of Coke Zero at the local Wellcome market, and we eagerly waited
for the contest to begin.

Each year the problem to be solved is revealed as the contest begins. The type
of problem varies by year. This year's problem was to write a program that
claimed edges on a graph. The goal was to construct a path from a "mine" node
to a set of "customer" nodes. Scoring was based on the number of customers
served.

After reading the problem description, we had to decide what programming
language to use. We were originally considering Python. But because the
problem had strict execution time limits, we decided that it would be better
to use a faster running language.

I decided to go with [Go](https://golang.org/), for the following reasons:

* I knew Go and its libraries well.
* Go runs well both on MacOS (where we were programming) and on Linux (where the contest organizers would be judging the program.)
* Go has easy-to-use networking libraries.
* Go has easy-to-use type-safe JSON marshalling and unmarshalling code.
* There was the potential to use Go's goroutines for extra performance.

We got to work, reading the problem specification and starting to write the
basic program.

I had hoped to do pair programming, but that didn't work out. The time scale
wasn't right. I'd say "OK, now I'm going to write the structs we're going to
use to read in the JSON", and then I wouldn't say anything else for an hour,
while I was doing that. Imagine how boring that was for my son!

I also regret that the time pressure meant that I couldn't answer my son's
tangential questions -- "What's a graph?", "What's functional programming?",
"What's JSON stand for?". I should probably have given up on trying to
compete, and instead used the time to educate my son. D'Oh!

Friday night we got as far as communicating with the contest's servers and
reading in the problem's JSON configuration file. My son was helpful in
monitoring the contest web site and twitter for announcements. He also helped
read the spec and serve as a foil for debugging.

Saturday morning we got up early, had a quick breakfast, and got to work. We
made steady progress, although as is typical we lost a few hours to dumb
mistakes. The worst one was a stray printf that broke the offline mode of the
app.

My son installed an IRC client (his first time using IRC) and he enjoyed
monitoring the contest IRC channel. He helped other contestants overcome
issues that we had already encountered.

By lunchtime we had our first working program, that simply picked the first
unclaimed edge on the map. It did surprisingly well against some opponents on
some maps.

![Pizza](/assets/posts/2017-08-06-Team_Blue_Iris_ICFP_2017_Programming_Contest_Postmortem-pizza.jpg)

We ate delicious pizzas from the local [Maryjane Pizza](http://www.maryjanepizza.com/) restaurant, and then went back to work.

By 6 PM we had our final lightning round, which tried to create paths from
mines to consumers. It worked, but not very well. Still, good enough for the
lightning round.

We spent a few hours decompressing and researching graph algorithms to use in
the main contest, and then went to bed.

On Sunday morning (day 2 of the contest) we decided not to continue with the
main contest. Our reason for quitting was a combination of burnout and
disinterest, combined with the sense that some teams were doing much better at
solving the problem.

###  What went right

* We blocked off a whole weekend to compete in the contest.
* We had the contest VM installed and running before the contest began.
* The Go programming language worked well.
  * The JSON, networking, and IO libraries were great.
  * Very few platform-specific issues between MacOS and Linux
  * Go made it easy to call an OS API on Linux to fix the lamduct EAGAIN issue that caused much heartburn to teams using languages like Java.
  * Super fast compile time.
  * Makefile-less build. The entire build script was: go build
* My son was an eager, supportive teammate.
* Our Internet and computer hardware worked flawlessly.

###  What went wrong

* Go is not quite the right language for graph traversal contests.
  * Too difficult to develop abstractions for graphs.
  * Temptation to use slices and maps for everything, which works, and is fast, but is too low level. Going up a level of abstraction introduces a lot of boilerplate.
  * Go's manual error checking adds a lot of overhead to writing code on "contest time".
* The Sublime Go IDE experience has regressed since I've used Go seriously. The GoSublime plugin seems to have rotted away in the past few years.
* Using Go disenfranchised my son, who currently only knows Python and Java.
* Our small team couldn't afford to invest time in writing tools (visualizers and contest servers) that would have helped us.
* My wife, who is a puzzle solving expert, was away during the contest. I'm sure she would have been a huge help in algorithm development.

Also, as my son diplomatically put it, "the problem was not the most
interesting". I think we would have had more fun with a more competitive or
physics oriented problem. To be fair, this year's problem as roughly in the
middle of all the problems that have been presented. It's certainly more
interesting than 2010's or 2013's abstract math problems.

###  Conclusion

Despite giving up on the main contest, we had fun during the lightning
contest. We look forward to reading the results of the competition and reading
the other teams' postmortems. And we look forward to competing again in 2018!
