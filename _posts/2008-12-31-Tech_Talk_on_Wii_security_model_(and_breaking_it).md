---
date: 2008-12-31 14:33
tags: Wii,gaming,security
---

# Tech Talk on Wii security model (and breaking it)

A very thorough talk describing the Nintendo Wii game console security model
and the bugs and weaknesses that allowed the Wii to be compromised:
[Console Hacking 2008: Wii Fail](http://events.ccc.de/congress/2008/Fahrplan/events/2799.en.html)

In a nutshell, security is provided by an embedded ARM CPU that sits between the
CPU and the IO devices, and handles all the IO. The two main flaws were (a) A
bug in the code that compared security keys, such that it was possible to
forge security keys, and (b) secret break-once-run-everywere information was
stored un-encrypted in RAM, where it could be extracted using hardware
modifications.

There's a nice table at the end of the presentation showing a
number of recent consumer devices, what their security model was, and how long
it took to break them.

The PS3 is the only console that's currently unbroken.
The PS3's security model seems similar to the Xbox 360, but somewhat weaker.
But it remains unbroken. This seems to due to the existence of an official PS3
Linux port, which means most Linux kernel hackers are not motivated to hack
the PS3 security. (Only the ones who want full access to the GPU from Linux
are motivated, and only to the extent that they can access the GPU.)
