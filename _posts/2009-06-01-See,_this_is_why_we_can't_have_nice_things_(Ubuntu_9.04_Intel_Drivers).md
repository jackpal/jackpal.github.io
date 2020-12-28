---
date: 2009-06-01 03:43
tags: Linux
title: See, this is why we can't have nice things (Ubuntu 9.04 Intel Drivers)
---

A few years ago I tried Ubuntu and predicted it would become a serious
challenger to Windows, in about 18 months.

Well, it's about 18 months later, was I right?

Not exactly. Ubuntu seems to have stood still, if not actually
gone backwards. In particular, the newer releases have much worse sound and
video performance on my hardware (Intel CPU/GPU Mac Minis) than earlier
releases.

The sound driver issue is because Linux, in its typical
decentralized fashion, is trying to figure out how to provide a standard audio
subsystem, and has two or three competing standards that are duking it out.
Since they all suck it seems odd that people defend them so much. Just pick
one already.

The video driver issue is because Intel decided to take several
years to rewrite their Linux video driver stack, and Ubuntu decided to ship
the new broken drivers rather than continue to use the old unbroken drivers.
Very very lame on both Intel and especially Ubuntu's part.

And Phoronix's
performance tests show that the performance of Ubuntu has gone downhill
slightly over the last few releases. (With no offsetting user-visible feature
improvements.) So we see the problem's larger than just sound and video
drivers.

It's almost as if the Linux community doesn't _want_ to be
successful.

Microsoft must be laughing all the way to the bank on this one.
