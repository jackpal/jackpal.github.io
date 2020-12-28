
---
date: 2007-12-29 18:10
tags: Hacking, Wii
title: Nintendo Wii security defeated by the Tweezer Attack
---

According to a presentation at the [24th Chaos Communication
Congress](http://events.ccc.de/congress/2007/Fahrplan/track/Hacking/2279.en.html),
hackers have apparently been able to defeat the Nintendo Wii game console's
security system using tweezers to bypass the hardware memory protection.

The
way it works is that the Wii runs in two modes: a GameCube emulation mode,
which has access to just 1/8th of the total memory, and Wii mode, that has
access to all the memory.

Hackers had already figured out how to run their own
code in GameCube mode. So the trick was to run their code in GameCube mode,
then use the tweezers to short out the address lines to allow the hacker's
code to access parts of rest of the memory. By shorting different address
lines different portions of memory were made available. By collecting enough
shards they eventually mapped all of memory.

Apparently the Wii operating
system keeps its digital signature keys in this protected memory, and once the
digital signatures were found it was possible to sign and run homebrew code on
the Wii.

It is not clear to me whether the attack is a per-machine attack or a
break-once-run-everywhere attack.
