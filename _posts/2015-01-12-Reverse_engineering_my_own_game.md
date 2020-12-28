---
date: 2015-01-12 05:55
description: Using an emulator to remember details of my old Atari game Dandy.
image: dandy-emulator-screen-shot.png
tags: games,Atari,Dandy,iOS
title: Reverse engineering my own game
---

I've long-since misplaced the source code to my Atari 800 game Dandy Dungeon.
But thanks to the [Atari800MacX](http://www.atarimac.com/atari800macx.php)
emulator and the emulation scene, I've been able to play an emulated version
of my original game. That's been helpful for remembering all the little
details of gameplay.

![Dandy Emulator Screen Shot](/assets/posts/2015-01-12-Reverse_engineering_my_own_game-dandy-emulator-screen-shot.png)

For example, I was able to determine that the original game animated the
arrows at 15 Hz and the players and monsters at 7.5 Hz.

FWIW I think the emulator may be slightly incorrect about the HBLANK
processing emulation. I'm pretty sure that the color background for the 4th
line of text should be a different color from the color background of the 3rd
line of text.

The iOS version of the game is progressing -- the dual thumbstick virtual
controls work well.

The next step (and it's a big one) is going to be multiplayer support. GameKit
here I come.
