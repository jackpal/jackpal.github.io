---
date: 2007-10-07 16:19
tags: linux
title: Linux in a Windows-centric home network
---

I was a happy Microsoft employee for many years, and as a result, I run
multiple Windows Vista machines at home. My family and I are happy with the
system, especially the Vista Media Center / Xbox 360 combination that we use
as our Digital Video Recorder, so I'm in no hurry to try and replace my
Windows servers with a Linux ones.

This leaves my poor Ubuntu 7.10 Macbook as
something of the odd man out. Over the past few weeks I've been learning how
to configure it to work with my mostly Windows network.

## Wireless Networking

This worked out-of-the-box. If I recall correctly, I had more trouble
connecting to my home wireless network when running Apple Macintosh OS X 10.4.
Hah, for what it's worth, my wireless router is a Linksys router that's
running Linux, so effectively there's no Windows involved. But I wanted to
mention that wireless networking and Internet connectivity worked well out-of-
the-box.

## Connecting to a Windows Vista File Share

Here's where I ran into my
first problem. It turns out that there are multiple ways of connecting to a
Windows server in Linux/Ubuntu, and they don't all work reliably. I found that
the UI-based way, using the Ubuntu "Places" menu, didn't work for me. I could
connect to my windows server, and view the server's directories, but I
couldn't reliably read the files. Accessing files was very slow, and reading
large files would always time out.

I was able to access my Windows Vista
shares by following these instructions:

[Mounting Windows Shares in Ubuntu](http://note2.industriousone.com/mounting-windows-shares-ubuntu) <--
allowed me to read my Window shares.

[Permission issues with smb and cifs](http://ubuntuforums.org/archive/index.php/t-318943.html) <-- allowed me
to delete files on my shares.

The downside of the command-line approach is
that you don't get a nice icon on your desktop, you have to navigate to
/mnt/myshare/... yourself. But it's reliable. You can partially work around
this by creating a symbolic link from your desktop to your share. The reason
this is a partial work-around is that Nautilus will think that the resulting
directory is a "local" directory, so it will try to do i/o intensive things
like create preview icons. Oh well.

For what it's worth, the reliability
problem with the default way of accessing Windows shares seems to be due to
[Ubuntu using the older, out-of-date smbfs system](http://joey.ubuntu-rocks.org/blog/2007/04/25/resolution-to-mounting-samba-shares-dont-use-smbfs/)
instead of the more modern cifs system. You'd think a hip, happening OS like
Ubuntu would fix this problem, but it's a long-standing one, so I guess it
hasn't made it to the top of their priority list yet.

# Windows Printing

This was easy.

1. First I shared out my printer on my Windows Vista machine. (I never bothered to do that before.)
2. Then on my Ubuntu machine I choose the System:Administration:Printing menu item.
3. Clicked on the New Printer icon
4. Chose "Windows Printer via SAMBA".
5. Fill in the dialog box. Use the handy "verify" button to verify that you've done it right.
6. Click on Forward and finish the configuration.

I was pleased to find my printer's model number mentioned in the driver list.
Everything worked the first time.
