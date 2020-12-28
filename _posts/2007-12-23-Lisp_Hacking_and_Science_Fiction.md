---
date: 2007-12-23 11:13
tags: lisp science_fiction
title: Lisp Hacking and Science Fiction
---

I've been poking around with Lisp and Scheme again, and am reminded of some of
my favorite science fiction books (warning, plot spoilers follow):

* Verner Vinge's "A Fire Upon the Deep" begins with a group of scientists mining an ancient civilization's web archives. They need to build interpreters for the ancient civilization's programs. All goes well until they reconstitute a malevolent AI that they spend the rest of the book fighting.
* Piers Anthony's Macroscope involves a group of people trying to decode an Extra-Terestrial message, that other ETs are trying to jam. Over the course of the book your opinion as to which group of ETs has humanity's best interests at heart changes back and forth several times.
* Any number of SF stories are set in the far future where people poke around in the ruins of a once-great civilization. (See Gene Wolfe, Cordwainer Smith.)

Working with Lisp reminds me of these books. Lisp's a seductive, ancient,
powerful language that has been worked on for years by very smart, very
motivated hackers. Pretty much everything one can think of to do with Lisp has
been done, multiple times, by really smart people.

For example, I want to have
a system where I can interactively write a game, changing code on the fly
while the game is running. I want to be able to use macros and garbage
collection and free serialization and inspection and all that cool stuff that
Lisp provides.

And Naughty Dog had all that, in GOAL, for the PS2. Their
implementation was apparently much better (more efficient, less buggy, more
features) than one I could cobble together out of open-source-Lisp parts. And
they had used it to successfully write two or three games. But they they
walked away from it. Gave it up. Went back to C++.

They said it was because
they had a lot of pressure from their parent company to make their engine more
reusable. But I think it's also because the results they were getting just
weren't that much better than the results that all the other developers, who
use C++-and-some-cheap-scripting-language-like-Lua, were getting.

I think Lisp
is like a powerful alien technology that may not be in your best interests to
use.
