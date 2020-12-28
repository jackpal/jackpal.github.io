---
date: 2016-12-21 17:43
tags: Home_computing Scooter_Computer hardware_trouble
title: Overheating fanless Scooter Computer
---

A few months ago, inspired by [The Scooter Computer](https://blog.codinghorror.com/the-scooter-computer/), I bought
[a fanless Intel Broadwell 5257u](https://www.aliexpress.com/item/Cheap-Fanless-Barebone-i5-i3-Mini-PC-Haswell-PC-Intel-Core-i5-4258U-i3-4158U-4K/32707085233.html?ws_ab_test=searchweb0_0,searchweb201602_1_116_10065_117_10068_114_115_113_10084_10083_10080_10082_10081_10060_10061_10062_10056_10055_10054_10059_10099_10078_10079_427_10103_10073_10102_10096_10052_10050_10051,searchweb201603_4,afswitch_5&btsid=91bf00dd-e88c-4a8f-b48e-e0fe679919c7)
computer. I set it up as a Core OS
Docker host, but quickly discovered that I didn't actually have much use for
Docker or Core OS. Recently I tried to repurpose it as a low-end Windows 10
game box. That unfortunately ran into heat-related stability issues. Windows
10 worked fine as long as I didn't try playing 3D games. Playing 3D games like
Counterstrike GO would crash after 10 to 15 minutes, with a very hot heat
sink. Fiddling with BIOS settings and drivers didn't make a difference.

I guess the lesson to learn from this is, when buying a fanless computer, not
to get the highest powered available CPU. Safer to get the lowest power CPU
that meets your performance needs. Or just get a fan. :-P
