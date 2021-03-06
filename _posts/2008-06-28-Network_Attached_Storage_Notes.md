---
date: 2008-06-28 19:03
tags: nas linux home_computing
title: Network Attached Storage Notes
---

I just bought a
[Buffalo LinkStation Mini 500GB](http://www.buffalotech.com/products/network-storage/linkstation/linkstation-mini/)
Networked Attached Storage (NAS) device. It's a very small
fanless Linux file server with two 250 GB hard drives, 128 MB of RAM, a 266
MHz ARM CPU and a gigabit Ethernet port.

## My reasons for buying a NAS

* I wanted to provide a reliable backup of family photos and documents, and I was getting tired of burning CDs and DVDs.
* I wanted a small Linux-based server I could play with.

## My reason for buying the LinkStation Mini

* It's fanless.
* It's tiny.
* Buffalo has a good reputation for NAS quality.
* There is a decent sized Buffalo NAS hacking community.
* Fry's had it on sale. :-)

## Setting it up

Setup was very easy -- I unpacked the box, pluged everything in, and installed
a CD of utility programs. The main feature of the utility program is that it
helps find the IP address of the NAS. All the actual administration of the NAS
is done via a Web UI.

## To RAID or not to RAID

The LinkStation Mini comes with two identical drives, initially set up as
RAID0. This means that files are split across the two drives, which means that
if either drive fails all your files will be lost. Using the Web UI, I
reformatted the drives to RAID1, which means that each file is stored on both
drives. This of course halves the amount of disk space available to store
files, but I thought the added security was worth it. This process of
switching over was fairly easy to do, but it erases all the data on the drives
and it takes about 80 minutes.
RAID1 is more secure than RAID0, but it is not perfectly secure. There's still
a chance of losing all the data if the controller goes bad, or if the whole
device is stolen or destroyed. So for extra security I will probably end up
buying a second NAS (or USB 2.0 drive), and setting up an automatic backup of
the backup device. The Mini can be set to perform periodic automatic backups
to a second LinkStation for this very reason. Once I do that, I'll probably
reformat my NAS's drives back to RAID0 to enjoy the extra storage space.


## Getting Access to Linux root

There is a program called [acp_commander](http://buffalo.nas-
central.org/index.php/Open_Stock_Firmware), that enables you to remotely log
in as root on any Buffalo LinkStation Mini on the same LAN as your PC. Once
logged in as root you can read and write any file on the NAS. You can use this
power to install software and reconfigure your system.

**Yes, this is a security hole -- it means anyone with access to your local
LAN can bypass all the security on the file server.**
[Very advanced users can patch the security hole by following the instructions at this web forum.](http://buffalo.nas-central.org/forums/viewtopic.php?f=37&t=6806)
I think it's extremely negligent of Buffalo to configure their NAS devices in
this way. Imagine the uproar if Microsoft shipped a product with this kind of
security hole.

## Playing with Linux

Once I obtained root access to the Mini I was able to install additional
software. I installed the
[Optware](http://www.nslu2-linux.org/wiki/MSSII/HomePage) package system,
which gives access to a wide variety of precompiled utility programs, as well
as tools for writing new programs.
(Yeah, I know, it's crazy to run software on a file server that's supposed to
be backing up important data. Right now I'm just having fun playing with my
new toy, but eventually I'm going to have to get serious about making it work
reliably.)

From looking at what other people have done, I am thinking that I might set up
a small web server, or perhaps a media server for streaming music and video.


## Thinking of the Future

There's an active LinkStation hacking community at
[buffalo.nas-central.org](http://buffalo.nas-central.org/). Unfortunately the Linkstation
Mini is so new that
[nobody in the NAS hacking community knows much about it.](http://buffalo.nas-central.org/forums/viewtopic.php?f=61&t=9156)
Right
now it seems to be [similar to a LinkStation Pro Duo](http://buffalo.nas-central.org/forums/viewtopic.php?f=61&t=7776),
but only experience will show
if this is true.
The Mini comes with a USB 2.0 port, to which you can attach a printer and/or a
hard disk. While the hard disk isn't part of a RAID array, it could be used to
back up the RAID array, providing an additional layer of security.

## Alternatives

There must be 20 different NAS vendors, although many of them just repackage
reference designs made by the SOC vendors. SOC mean System on Chip. Marvell
seems to be the dominant player in the NAS SOC market these days. A good
overview of available NAS products can be found by visiting
[Small Net Builder](http://www.smallnetbuilder.com/component/option,com_nas/Itemid,190).
Some brands like [Revolution](http://www.revogear.com/),
[QNAP](http://www.qnap.com/) and
[Synology](http://www.synology.com/enu/products/index.php) cater to
enthusiasts who are interested in using the NAS as a mini Linux server. The
only thing that stopped me from buying those brands is that (a) they're more
expensive, and (b) they don't currently have fanless RAID1 form factors.
The Revolution brand is actually owned by Buffalo. They add hardware daughter
boards to standard Buffalo products. The daughter boards have extra flash
chips and I/O connectors. It's possible that there will be a Revolution "Kuro
box" version of the Mini some day.

The venerable (out-of-production, but still available in stores) [Linksys
NSLU2](http://www.nslu2-linux.org/) product is fanless and cheap, and [very
popular with hackers](http://www.nslu2-linux.org/), but you need to add hard
drives, and I don't think its networking performace is very good compared to
more recent products.

Another approach is to use a PC, either running a regular OS like Windows XP,
Windows Server, OSX or Linux, or a special-purpose stripped-down NAS version.
I do have an old PC currently running Windows Media Center that I could use
for this purpose, but I didn't seriously consider this option because I wanted
something small, low-power, and quiet. (And I was looking for an excuse to
learn how to administer a Linux system anyway.)

Apple makes NAS products too. Their the Airport Extreme and Time Capsule
products both look OK, but neither one supports RAID1. And there doesn't seem
to be a software hacking community around these products. There is a software
hacking community around the AppleTV, which you could make into a NAS by
adding some USB 2.0 hard drives.

Some routers (like the Apple Airport Extreme mentioned above) have USB 2.0
ports, but I think they avoid advertising themselves as NAS products because
they don't have enough RAM (or CPU) to act as both routers and file servers.
As a result, these products tend to have relatively low NAS performance.

Some people would laugh at a NAS that has only 240GB of storage. They are more
interested in the high-end NASes that use four or five 1GB disks. When
formatted in RAID5 configuration those NASes have 3GB of usable space. But
they also cost $600 plus the cost of the drive ($160 each). Which is much more
than I wanted to spend. Besides the cost, another drawback is that these
products are nearly as large and noisy as regular PCs. Still, if you've got a
lot of video (or are anticipating generating a lot of video in the future) the
larger NASs are the way to go.


## A NAS in Every Garage?

While all my friends and I are setting up file servers to store their family's
videotapes, I'm not sure if the product will become universally popular. I
think it will depend on how people's secure storage needs evolve.
We're already seeing small files (email, photos, low-res videos) being stored
in the cloud. It seems like it's just a matter of time before everything is.
Unless people suddenly come up with compelling new applications that use
dramatically more data (holographic TV perhaps?), it seems likely that
people's personal storage needs are going to top out in the next decade. If
disk capacity and network bandwidth keep growing at a rapid pace for several
decades beyond that, then it seems inevitable that cloud storage will
eventually take over.

In any event, by the time this happens my little Mini will long since have
been retired. (I remember paying $100 apiece for 1GB Jaz disks back in the
day. It's amazing how far and how fast storage prices have fallen.) If all
goes well, my my family's photos and other important documents will still be
around!
