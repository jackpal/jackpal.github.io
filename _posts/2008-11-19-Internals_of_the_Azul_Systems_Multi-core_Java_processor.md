---
date: 2008-11-19 13:24
tags: Java Hardware Azul Systems
title: Internals of the Azul Systems Multi-core Java processor
---

I'm a big fan of CPU architectures. Here's a conversation between David Moon
formerly of Symbolics Lisp Machines and Cliff Click Jr. of Azule Systems. They
discuss details of both the Lisp Machine architecture and Azule's massively
multi-core Java machine.

[A Brief Conversation with David Moon](http://blogs.azulsystems.com/cliff/2008/11/a-brief-conversation-with-david-moon.html)

The claim (from both Symbolics and Azule)
is that adding just a few instructions to an ordinary RISC instruction set can
make GC much faster. With so much code being run in Java these days I wonder
if we'll see similar types of instructions added to mainstream architectures.
