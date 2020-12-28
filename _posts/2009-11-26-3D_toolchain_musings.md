---
date: 2009-11-26 23:09
tags: 3d_graphics android
title: 3D toolchain musings
---

I'm writing a skinning sample for a future Android SDK. This has prompted me
to construct a toolchain to get skinned animated models into Android
applications.

I'm really only just getting started, but so far I'm thinking along these
lines:

Wings 3D -> Blender 2.5 -> FBX ASCII -> ad-hoc tool -> ad-hoc binary file ->
ad-hoc loader.

For what it's worth, right now Blender 2.5 doesn't support FBX export, so I
have to do Collada -> FBX Converter -> FBX. But Blender 2.5 support should be
ready fairly soon.

You may wonder why I'm considering using the proprietary and undocumented FBX
standard rather than the open source Collada standard. I'm doing it for the
same reason so many other tool chains (e.g. Unity, XNA) do, namely that FBX is
a simpler format that just seems to work more reliably when exchanging data
between DCC applications.
