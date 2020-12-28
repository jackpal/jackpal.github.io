
---
date: 2009-03-27 15:23
tags: xbmc,Linux,Mac
title: Using XBMC on Mac Mini using both OSX and Linux
---

The Xbox Media Center (XBMC) is a nifty open-source application for watching
videos. It was originally designed for use on modified Xbox video game
consoles, but has more recently become popular for Intel-based Home Theater
Personal Computers. It has been ported to Windows, Mac, and Linux. It has no
PVR features, instead it concentrates on displaying streaming and downloaded
videos. Its big advantage over using the Xbox 360's similar application is
that it handles a much wider variety of streaming video sources and downloaded
video codecs.

I've been running [Plex](http://www.plexapp.com/), an OSX-
specific version of the Xbox Media Center, on my Mac Mini for several months
now. Overall it's a good product, but I had some issues for my application. I
wanted Plex to serve as a consumer electronic device that my mother-in-law
(who doesn't use computers and can't read English) could use by herself to
watch videos. The system I put together didn't work very well for her. The
problems we ran in to were:

1) The integration with the 6-button Apple Remote
Control into the Plex/XBMC UI leaves a lot to be desired. The XBMC UI was
designed to be used with a full-featured remote, and the Apple Remote mapping
is just too hard to use. My mother-in-law would end up in the weeds of some
obscure corner of the Plex UI, without knowing how she had gotten there or how
to get back. The Plex software contributed to this problem by having a very
sluggish interface to the Apple Remote, that frequently missed clicks. When
you couple this with the overloading of "short" and "long" presses to try and
give the Apple Remote more logical buttons, it became quite difficult (even
for me) to reliably drive the UI. Even a task as simple as backing out of a
playing video was difficult to do reliably.

2) OSX (and Plex) have trouble
running in consumer-electronics mode, without a keyboard or mouse. OSX and
Plex both liked to bring up modal dialogs to report errors or software
updates. I was always having to drag out a keyboard and mouse to dismiss these
dialogs.

Now, a sensible person would work around these issues by buying a
Bluetooth keyboard and mouse, and software like
"[Remote Buddy](http://www.iospirit.com/index.php?mode=view&obj_type=infogroup&obj_id=24&sid=9214373G9cd3975527aa3499)"
that enables the use of a full-featured remote. A somewhat more ambitious
person might have rescripted the Plex UI to work better with the Apple Remote,
or even dug into the sources to try and fix the sluggish event problem. But
I'm restless, and wanted an excuse to try out Linux on the Mac Mini anyway. So
this week I decided to see if the Linux version of XBMC worked any better.

### Installing Linux XBMC

Installing Linux is alot like the old pre-Windows 95
days of DOS. I spent a lot of time trying different things and fiddling with
hardware issues. Here's what finally worked for me:

* Installed [rEFIt](http://refit.sourceforge.net/) to allowing easy dual-booting between Mac and Linux
* Installed [Ubuntu 8.10 32-bit](http://releases.ubuntu.com/8.10/) version.
* [Compiled XBMC from the tip-of-tree svn sources](http://xbmc.org/wiki/?title=HOW-TO_compile_XBMC_for_Linux_from_source_code).
* Installed the LIRC infrared remote control driver (apt-get install lirc) and used an old USB-based Windows Media Center remote control that I already had.

So far (one day) this has worked well. The full-functioned remote control make
a big difference in usability.

## Some issues I ran into

### Ubuntu 9.04 beta problems with OpenGL accelleration for the Mac Mini

The Ubuntu 9.04 beta Intel
945 OpenGL driver does not hardware accelerate as many features of OpenGL as
in older versions of Ubuntu. XBMC's user interface runs very slowly. This is
not XBMC-specific. Try using apt-get to install the "amoeba" OpenGL demo. It
runs smoothly on Ubuntu 8.10, but is a 2-frame-per-second slide-show on Ubuntu
9.04 beta. I hope this regression gets fixed in future versions of Ubuntu
9.04, as it otherwise looks like a good system.

### The prebuilt "PPA" XBMC binaries will crash on Ubuntu 8.10 when pausing video

I had to build XBMC from
the subversion sources in order to fix a bug where pausing a video would
immediately cause XBMC to crash. (I used a build from Thursday March 26th. I'm
sorry but this is the first time I've used subversion, so I don't know how to
figure out which revision number I'm synced to.) This is a bug that's been
reported several times in the XBMC forums. It seems to be solved by compiling
from source, without making any other changes. I'm suspicious that this may be
due to some subtle difference between the libraries that you install to
compile and the libraries that are installed when you install the prebuilt
binary. (But that's just a guess. The real reason may be something completely
different.)

Well, after all this the system seems to work pretty well for my
application. Too bad my mother-in-law's finished her visit with us and gone
back home. At least now I've got plenty of time to work out the bugs before
her next visit.

[Revision notes]

3/27/09 - Updated for Ubuntu 9.04 beta.
