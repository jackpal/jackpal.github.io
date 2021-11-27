---
date: 2021-11-27
tags: Apple M1 console
title: Why Apple is Unlikely to Create a Game Console
---

# Why Apple is Unlikely to Create a Game Console

Apple's new M1 Max SOC has good graphics performance. Some people are speculating that this would enable Apple to create a competitive video game console. I think that's unlikely.

The bull case for Apple making a video game console is:

+ Apple has designed a series of excellent SOCs to serve their existing product lines.
+ The recent Macbook Pro M1 Max SOC has performance that is comparable to AMD/NVIDIA laptop gaming SOCs, but at lower power consumption.
+ Apple could use the M1 Max SOC to create a video game console with similar performance to existing video game consoles, but with much lower power requirements.
+ This would enable Apple to compete in the console video game market.

The bear case is:

- Apple doesn't care about the console video game market. It's small and not growing.
- Video game console customers don't value the benefits that Apple's M1 Max SOC would bring to video games.

In this blog post I explain why the video game console market is shaped the way it is.

<!--more-->

Here's why video game consoles are cheap, & hot:

+ People have a wide variety of choices in how they play games. Most people will play the best games available out of the possible choices.
+ Everyone has a smart phone, with an endless supply of pretty good games. This limits the video game console market to game experiences that are not possible or not enjoyable on phones.
+ Improving graphics (and audio) is a reliable, effective way of differentiating a console game from a mobile game.
+ TVs are increasing in resolution every console generation. While TV resolution is in the era of diminishing returns, customers are for the moment still want to buy higher resolution TVs every console generation.
+ Higher res TVs require 4x the GPU processing power (and RAM) each generation.
+ As a result, the video game console customer tends to value a console with high CPU and GPU power, lots of RAM.
+ Using a fixed performance profile (fixed for the life of the console) significantly increases available performance by allowing developers to do all sorts of microoptimizations that would not be feasible if there were differences between consoles.
+ Video game consoles are in production over 5-7 years. Over that time some of their components may get cheaper (CPU, RAM), others may just get higher capacity at the same price (spinning hard disks). Others may stay the same cost (power supplies). And the CPU & RAM typically switch to better / cooler running processes.
+ Console vendors can (and do) make yearly updates to the hardware, swapping components to cost reduce the product. This is planned into the console design from the beginning.
+ Sales increase yearly over the life of the console (as the price is brought down, and as the catalog of games increases and network effects kick in.)
+ People have a fixed upper bounds on the price they are willing to spend up front, and similar upper bounds on cost-per-game and cost-per-month. This establishes an expected lifetime value of a console purchaser, which sets the limit on bill-of-materials. (Although you can segment the market, like Xbox S / Xbox X.)

Those facts together mean you want to design your console so that it's as powerful as possible for a given price.

+ For a given CPU design and silicon process, you can usually increase performance by increasing the voltage.
+ Increasing the voltage increases the heat output.
+ Air cooling remains the cheapest cooling solution.
+ It's hard to move a lot of air quietly.
+ Hardware designers are almost always optimistic about power/heat. Consoles typically end up generating more heat than their designers expected. When that happens, all you can do is crank up the fan.
 
Put all this together, and it's natural for the first year's version of a console design to be as hot and noisy as possible. We'll eventually see consoles top out at a limit of ≈1200 watts, due to US household electricity codes.

Given all this, Apple's theoretical cool-and-quiet-and-expensive device is going to have to compete against hot-and-loud-and-cheap devices. Most consumers are going to choose the cheap devices.