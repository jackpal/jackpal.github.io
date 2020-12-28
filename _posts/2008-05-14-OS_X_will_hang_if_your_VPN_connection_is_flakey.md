
---
date: 2008-05-14 07:04
title: OS X will hang if your VPN connection is flakey
---

OS X is by and large a good OS, but once you get past the sexy UI you find a
lot of rough edges.

For example, this month I've been working remotely over a
flakey DSL connection. I ran into a very frustrating problem: if you're using
a PPTP-based VPN, and your network connection is poor quality, the whole Apple
UI will frequently freeze up with the "Spinning beachball" cursor for minutes
at a time.

Luckily for me the work-around is to reboot my DSL modem. But it
seems like poor system design for the VPN packet performance to affect the UI
of non-networked applications.
