---
date: 2010-03-06 20:17
tags: contests go C++ JavaScript
title: Google AI Challenge 2010
---

[Team Blue Iris](http://csclub.uwaterloo.ca/contest/profile.php?user_id=1515)
(that's me!) placed 77th out of 708 in the
[Google AI Challenge 2010](http://csclub.uwaterloo.ca/contest/) contest. The contest was to design
an AI for a Tron lightcycle game.

My entry used the standard min/max approach, similar to many other entries. I
guess that's why it performed similarly to so many other entries. :-) I didn't
have any time to investigate better algorithms, due to other obligations.

In addition to my own entry (which was in C++), I also created starter packs
for JavaScript and Google's Go language. These starter packs enabled people to
enter the contest using JavaScript or Go. In the end,
[6 people entered using Go](http://csclub.uwaterloo.ca/contest/language_profile.php?lang=Go), and
[4 people entered using JavaScript](http://csclub.uwaterloo.ca/contest/language_profile.php?lang=JavaScript).
I'm pleased to have helped people compete using these languages.

I initially wanted to use JavaScript and/or Go myself, but the strict time
limit of the contest strongly favored the fastest language, and that made me
choose a C++ implementation. Practically speaking, there wasn't much advantage
to using another language. The problem didn't require any complicated data
structures, closures, or dynamic memory allocation. And the submitted entries
were run on a single-hardware-thread virtual machine, which meant that there
wasn't a performance benefit for using threads.

This contest was interesting because it went on for a long time (about a
month) and because the contestants freely shared algorithm advice during the
contest. This lead to steadily improving opponents. My program peaked at about
20th place early in the contest, but because I didn't improve it thereafter,
its rank gradually declined over the rest of the contest.

[The contest winner's post-mortem](http://a1k0n.net/blah/archives/2010/03/index.html)
reveals that he won by diligent trial-and-error in creating a superior evaluation function. Good
job a1k0n!

Congratulations to the contest organizers for running an interesting contest
very well. I look forward to seeing new contests from the University of
Waterloo Computer Club in the future.
