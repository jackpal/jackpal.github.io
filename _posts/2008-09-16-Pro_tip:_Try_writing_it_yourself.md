---
date: 2008-09-16 14:28
tags: productivity
title: Pro tip: Try writing it yourself
---

Sometimes I need to get a feature into the project I'm working on, but the
developer who owns the feature is too busy to implement it. A trick that seems
to help unblock things is if I hack up an implementation of the feature myself
and work with the owner to refine it.

This is only possible if you have an
engineering culture that allows it, but luckily both Google and Microsoft
cultures allow this, at least at certain times in the product lifecycle when
the tree isn't frozen.

By implementing the feature myself, I'm (a) reducing
risk, as we can see the feature sort of works, (b) making it much easier for
the overworked feature owner to help me, as they only have to say "change
these 3 things and you're good to go", rather than having to take the time to
educate me on how to implement the feature, (c) getting a chance to implement
the feature exactly the way I want it to work.

Now, I can think of a lot of
situations where this approach won't work: at the end of the schedule where no
new features are allowed, in projects where the developer is so overloaded
that they can't spare any cycles to review the code at all, or in projects
where people guard the areas they work on.

But I've been surprised how well it
works. And it's getting easier to do, as distributed version control systems
become more common, and people become more comfortable working with multiple
branches and patches.
