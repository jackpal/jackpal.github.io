---
date: 2008-03-12 22:53
tags: Carmack, 3d
---

# Carmack speaks on real next-gen graphics

John Carmack is experimenting with a "sparse octree" data structure for
accelerating 3D graphics rendering:

[Carmack on Octrees](http://www.pcper.com/article.php?aid=532&type=overview)

Best quote:

> The direction that everybody is looking at for next generation, both console and
> eventual graphics card stuff, is a "sea of processors" model, typified by
> Larrabee or enhanced CUDA and things like that, and everybody is sort of
> waving their hands and talking about “oh we’ll do wonderful things with all
> this” but there is very little in the way of real proof-of-concept work going
> on. There’s no one showing the demo of like, here this is what games are going
> to look like on the next generation when we have 10x more processing power -
> nothing compelling has actually been demonstrated and everyone is busy making
> these multi-billion dollar decisions about what things are going to be like 5
> years from now in the gaming world. I have a direction in mind with this but
> until everybody can actually make movies of what this is going to be like at
> subscale speeds, it’s distressing to me that there is so much effort going on
> without anybody showing exactly what the prize is that all of this is going to
> give us.

Second-best quote is that he wants lots of bit-twiddling operations-
per-second to traverse the data structures rather than lots of floating-point-
operations-per-second. Must be scary for the Larrabee and NVIDIA CPU
architects to hear that, this late in their design cycles.

Hopefully John will come up with a cool demo that helps everyone understand whether his approach
is a good one or not. Hopefully the Larrabee / NVIDIA architectures are
flexible enough to cope. (Interestingly, no mention of ATI -- have they bowed
out of the high-end graphics race?)
