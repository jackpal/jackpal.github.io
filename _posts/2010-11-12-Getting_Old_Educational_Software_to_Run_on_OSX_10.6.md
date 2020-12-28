---
date: 2010-11-12 23:25
tags: pc,educational,OSX
---

# Getting Old Educational Software to Run on OSX 10.6

My kids' favorite piece of educational software is
"[Clifford The Big Red Dog Thinking Adventures](http://www.google.com/search?q=Clifford+thinking+adventures)" by
Scholastic. We received it as a hand-me-down from their cousins. The CD was
designed for Windows 95-98 and pre-OSX Macintosh. Unfortunately, it does not
run on OS X 10.6. (I think it fails to run because it uses the PowerPC
instruction set and Apple dropped support for emulating that instruction set
in 10.6.)

After some trial and error, I found the most reliable way to run Clifford on
my Mac was:

1. Install [VMWare Fusion](http://www.vmware.com/products/fusion/).
2. Install [Ubuntu 10.10 32-bit](http://www.ubuntu.com/desktop/get-ubuntu/download) in a Virtual Machine.
3. Install the VMWare Additions to make Ubuntu work better in the VM.
4. [Install Wine on Ubuntu 10.10](http://www.multimediaboom.com/how-to-upgarde-to-latest-wine-1-3-5/). (Wine provides partial Windows API emulation for Linux.)
5. Configure the audio for Wine. (I accepted the defaults.)
6. Insert the Clifford CD and manually run the installer.

The result is that the Clifford game runs full screen with smooth animation
and audio, even on my lowly first-generation Intel Mac Mini.

You may scoff at all these steps, but (a) it's cheap, and (b) it's less work
than installing and maintaining a Windows PC or VM just to play this one game.

(It's too bad there isn't a Flash, HTML5, Android or iOS version of this game,
it's really quite well done.)
