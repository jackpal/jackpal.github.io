---
date: 2007-10-21 18:58
tags: linux macbook
title: Customizing Ubuntu 7.10 on Macbook
---

I've been tearing down and reinstalling Ubuntu 7.10 all weekend, trying to get
wireless video playback to work well. Here's my list of tweaks, all of which
are unrelated to wireless video playback:

1. I personally like the Edubuntu 7.10 distribution more than the stock Ubuntu distribution. Edubuntu has a nicer default visual theme and some nice educational games.
2. Apply customizations from the [MacBook Ubuntu Forum](https://help.ubuntu.com/community/MacBook#head-7d017a785b4c25105fbb19b19a1ef3ec984d7f54)
  1. I just set up my keyboard so that the lower Enter button acts as the right mouse button.
3. Make text better:
  1. System:Preferences:Appearence:Fonts:Subpixel Smoothing
4. The Google Toolbar for Firefox has a bug where bookmarks won't load. A work-around is to [use the Synaptic Package Manager to install libstdc++5 and its dependencies](http://ubuntuforums.org/showpost.php?p=3595161&postcount=7).
5. In order to get the keyboard "Mute" button to work, open System:Preferences:Sound and select all the channels in the "Default Mixer Tracks" list. (Hold down the Control key while clicking on each channel.)
