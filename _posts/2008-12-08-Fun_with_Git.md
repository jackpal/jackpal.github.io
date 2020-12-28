---
date: 2008-12-08 18:35
tags: git hobby programming
title: Fun with Git
---

This weekend I reorganize my home source code projects. I have a number of
machines, and over the years each one had accumulated several small source-
code projects. (Python scripts, toy games, things like that.) I wanted to put
these projects under source code control. I also wanted to make sure they were
backed-up. Most of these little projects are not ready to be published, so I
didn't want to use one of the many web-based systems for source-code
management.

After some research, I decided to use replicated git repositories.

I created a remote git repository on an Internet-facing machine, and then
created local git repositories on each of my development machines. Now I can
use git push and git pull to keep the repositories synchronized. I use git's
built-in ssh transport, so the only thing I had to do on the Internet-facing-
machine was make sure that the git executables were in the non-interactive-
ssh-shell's path. (Which I did by adding them in my .bashrc file.)

Git's
ability to work off-line came in handy this Sunday, as I was attending an
elementary-school chess tournament with my son. Our local public schools don't
have open WiFi, so there was no Internet connectivity. But I was able to
happily work away using my local git, and later easily push my changes back to
the shared repository.
