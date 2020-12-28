---
date: 2015-07-26 15:58
description: Fun with a classic 3D renderer.
image: elephant.jpg
tags: renderman 3d macos
title: Pixar Non-Commercial Renderman for OS X
---

Pixar released their Non Commercial version of Renderman. Woo! Hurray for
patent expiration dates!

If you don't have Maya and you do have Mac and you just want to play with the
command-line version, you have to jump through several hoops:

You need to install [XQuartz](http://xquartz.macosforge.org/) before trying to
install Renderman. (Otherwise the Renderman installer will fail.)

You need to copy /Applications/Pixar/RenderManProServer-19.0/etc/rendermn.ini
to ~/.rendermn.ini  (Note the added "." in the front.)

You need to edit .rendermn.ini to add the line

```
    /licenseserver    ${RMANTREE}/../pixar.license
```

You need to add these lines to your bashrc (or equivalent, depending on your
shell.)

(Found on <https://www.fxphd.com/kbslug/renderman/>)


```shell
export RMANTREE=/Applications/Pixar/RenderManProServer-19.0/

export DYLD_LIBRARY_PATH=/Applications/Pixar/RenderManProServer-19.0/lib/

export RMANFB=it

export export

PATH=$PATH:$RMANTREE/bin:$RMSTREE/bin/it.app/Contents/MacOS:$RMSTREE/bin/slim.app/Contents/MacOS
```

Once you've done that, you can use the "prman" command line tool to render rib
files.

![Renderman Elephant Carving](/assets/posts/2015-07-26-Pixar_Non-Commercial_Renderman_for_OS_X-elephant.jpg)
