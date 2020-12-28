---
date: 2009-01-08 04:32
tags:Renderman, Pixar,3d
---

# "Pixar Quality Graphics" is 720 Gflops

For at least 10 years GPU vendors have been talking about "Pixar Quality"
graphics. But what does that mean? Well, according to this lecture on
[The Design of Renderman](http://graphics.stanford.edu/courses/cs448a-01-fall/lectures/lecture16/walk002.html),
the original goals for the REYES architecture were

* 3000 x 1667 pixels (5 MP)
* 80M Micropolygons (each 1/4th of a pixel in size, depth complexity of 4)
* 16 samples per pixel
* 150K geometric primitives
* 300 shading flops per micropolygon
* 6 textures per primitive
* 100 1MB textures

That's a shading rate of 80M * 300 * 30 Hz = 720 Gflops. (They were probably
more concerned about 24 Hz, but for games 30Hz is better.)

In comparison I
think the peak shader flops of high-end 2008-era video cards are in the 1
TFlop range. (Xbox 360 Xenos is 240 Gflops, PS3 is a bit less.). Now, GPU
vendors typically quote multiply-accumulate flops, because that doubles the
number of flops. So it's more realistic to say that 2008-era video cards are
in the 500 Gflop range. So we really are entering the era of "Pixar Quality"
graphics.
