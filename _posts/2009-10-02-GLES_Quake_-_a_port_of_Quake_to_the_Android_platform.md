---
date: 2009-10-02 02:48
tags: opengl_es quake opengl android
title: GLES Quake - a port of Quake to the Android platform
---

I've just published the source code to
[a port of Quake to the Android platform](http://code.google.com/p/glesquake).
This is something I did a while ago as a internal test application.

It's not very useful as a game, because there hasn't been any attempt to
optimize the controls for the mobile phone. It's also awkward to install
because the end user has to supply the Quake data files on their own.

Still, I thought people might enjoy seeing a full-sized example of how to
write a native code video game for the Android platform. So there it is, in
all its retro glory.

(Porting Quake II or Quake III is left as an exercise for the reader. :-)

What's different about this particular port of Quake?

### Converted the original application into a DLL

Android applications are written in Java, but they are
allowed to call native languge DLLs, and the native language DLLs are allowed
to call a limited selection of OS APIs, which include OpenGL ES and Linux File
I/O.

I was able to make the game work by using Java for:

* The Android activity life-cycle
* Event input
* View system integration
* EGL initialization
* Running the game in its own rendering loop, separate from the UI thread

And then I use C++ for the rest. The interface between Java and C is pretty
simple. There are calls for:

* initialize
* step - which does a game step, and draws a new frame.
* handle an input event (by setting variables that are read during the "step" call.)
* quit

The Java-to-C API is a little hacky. At the time I developed this game I
didn't know very much about how to use JNI, so I didn't implement any C-to-
Java calls, preferring to pass status back in return values. For example the
"step" function returns a trinary value indicating:

1. That the game wants raw PC keyboard events, because it is in normal gameplay mode.
2. That it wants ASCII character events, because it is in menu / console mode.
3. That the user has chosen to Quit.

This might better have been handled by providing a series of Java methods that
the C code could call back. (But using the return value was easier. :-))

Similarly, the "step" function takes arguments that give the current screen
size. This is used to tell the game when the screen size has changed. It would
be cleaner if this was communicated by a separate API call.

### Converted the Quake code from C to C++

I did this early in development, because I'm more
comfortable writing in C++ than in C. I like the ability to use classes
occasionally. The game remains 99% pure C, since I didn't rewrite very much of
it. Also, as part of the general cleanup I fixed all gcc warnings. This was
fairly easy to do, with the exception of aliasing warnings related to the
"Quake C" interpreter FFI.

### Converted the graphics from OpenGL to OpenGL ES

This was necessary to run on the Android platform. I did a straightforward
port. The most difficult part of the port was implementing texture conversion
routines that replicated OpenGL functionality not present in OpenGL ES.
(Converting RGB textures to RGBA textures, synthesizing MIP-maps and so
forth.)

### Implemented a Texture Manager

The original glQuake game allocated
textures as needed, never releasing old textures. It relied upon the OpenGL
driver to manage the texture memory, which in turn relied on the Operating
System's virtual memory to store all the textures. But Android does not have
enough RAM to store all the game's textures at once, and the Android OS does
not implement a swap system to offload RAM to the Flash or SD Card. I worked
around this by implementing my own ad-hoc swap system. I wrote a texture
manager that uses a memory mapped file (backed by a file on the SD Card) to
store the textures, and maintained a limited size LRU cache of active textures
in the OpenGL ES context.

### Faking a PC Keyboard

Quake expects to talk to a PC keyboard as its primary
input device. It handles raw key-down and key-up events, and handles the shift
keys itself. This was a problem for Android because mobile phone devices have
a much smaller keyboard. So a character like '[' might be unshifted on a PC
keyboard, but is shifted on the Android keyboard.

I solved this by translating
Android keys into PC keys, and by rewriting the config.cfg file to use a
different set of default keys to play the game. But my solution is not
perfect, because it is essentially hard-coded to a particular Android device.
(The T-Mobile G1). As Android devices proliferate there are likely to be new
Android devices with alternate keyboard layouts, and they will not necessarily
be able to control the game effectively using a G1-optimized control scheme.

A better approach would be to redesign the game control scheme to avoid needing
keyboard input.

### A Few Words about Debugging

I did the initial bring-up of
Android Quake on a Macintosh, using XCode and a special build of Android
called "the simulator", which runs a limited version of the entire Android
system as a native Mac OS application. This was helpful because I was able to
use the XCode visual debugger to debug the game.

Once the game was limping
along, I switched to debugging using "printf"-style log-based debugging and
the emulator, and once T-Mobile G1 hardware became available I switched to
using real hardware.

Using printf-style debugging was awkward, but sufficient
for my needs. It would be very nice to have a source-level native code
debugger that worked with Eclipse.

Note that the simulator and emulator use a
software OpenGL ES implementation, which is slightly different than the
hardware OpenGL ES implementation available on real Android devices. This
required extra debugging and coding. It seems that every time Android Quake is
ported to a new OpenGL ES implementation it exposes bugs in both the game's
use of OpenGL ES and the device's OpenGL ES implementation.

Oh, and I sure
wish I had a Microsoft PIX-style graphics debugger! It would have been very
helpful during those head-scratching "why is the screen black" debugging
sessions.
