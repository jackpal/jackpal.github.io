---
date: 2010-09-06 02:42
tags: pc Home computing  decluttering
title: Eulogy for a PC
---

This Labor Day weekend I decommissioned my tower PC.

If memory serves, I bought it in the summer of 2002, in one last burst of PC
hobby hacking before the birth of my first child. It's served me and my
growing family well over the years, first as my main machine, later as a media
server.

But the world has changed, and it doesn't make much sense to keep it running
any more. It's been turned off for the last year while I traveled to Taiwan.
In that time I've learned to live without it.

The specific reasons for decommissioning it now are:

* It needs a new CMOS battery.
* It needs a year's worth of Vista service packs and security updates.
* I don't use the media center feature any more.
* After a year without a Windows machine I don't want to maintain one any more.
* It's too noisy and uses too much energy to leave on as a server.

**The decommissioning process**

I booted it, and combed through the directories. I carefully copied off all
the photos, code projects, email and documents that had accumulated over the
years. Lots of good memories!

**GMail Import**

I discovered some old Outlook Express message archives. I wrote a Python
script to import them into GMail. I didn't like any of the complicated recipes
I found on the web, so I did it the easy way: I wrote a toy POP3 server in
Python that served the ".eml" messages from the archive directory. Once the
toy POP3 server was running, GMail was happy to import all the email messages
from my server.

**Erasing the disks with Parted Magic**

Once I was sure I had copied all my data off the machine it was time to erase
the disks. I erased the disks using the
[ATA Secure Erase](https://ata.wiki.kernel.org/index.php/ATA_Secure_Erase) command. I did
this using a small Linux distro, [Parted Magic](http://partedmagic.com/). I
downloaded the distribution ISO file and burned my own CD. Once the CD was
burned, I just rebooted the PC and it loaded Parted Magic from the CD.

Parted Magic has a menu item, "Erase Disk". It lets you choose the disk and
the erase method. The last method listed is the one I used. The other methods
work too, but they don't use the ATA Secure Erase command.

It took quite a while to erase the disks. Each disk took about two hours to
erase. (They took 200 GB per hour, to be precise.) Unfortunately, due to the
limitations of my ATA drivers, I had to erase them one at a time.

While I think the ATA Secure Erase command is the fastest and most reliable
way to erase modern hard disks, there is another way: Instead of having the
drive erase itself, you can copy data over every sector of the drive. The
advantage of this technique is that it also works with older drives. If you
choose to go this route, one of the easiest way to do it is to use
[Darik's Boot and Nuke](http://www.dban.org/) utility. This is a Linux distro that does
nothing besides erasing all the hard disks of the computer you boot it on. You
just boot the CD, then type "autonuke" to erase every disk connected to your
computer.

**Disposing of the computer**

It's still a viable computer, so I will probably donate it to a local charity.
The charity has a rule that the computer you donate must be less than 5 years
old. I think while some parts of the computer are older than that, most of the
parts are young enough to qualify.

**Reflections on the desktop PC platform**

Being able to upgrade the PC over the years is a big advantage of the
traditional desktop tower form factor. Here's how I upgraded it over the
years:

Initial specs (2002)

* Windows XP
* ASUS P4S533 motherboard
* Pentium 4 at 1.8 GHz
* ATI 9700 GPU
* Dell 2001 monitor (1600 x 1200)
* 256 MB RAM
* 20 GB PATA HD
* Generic case


Final specs (2010)

* Windows Vista Ultimate
* ASUS P4S533 motherboard
* Pentium 4 at 1.8 GHz
* ATI 9700 GPU
* Dell 2001 monitor (1600 x 1200)
* 1.5 GB RAM
* 400 GB 7200 RPM PATA HD
* 300 GB 7200 RPM PATA HD
* ATI TV Theater analog capture card
* Linksys Gigabit Ethernet PCI card
* Antec Sonata quiet case


I think these specs show the problems endemic to the desktop PC market.
Although I didn't know it at the time, the PC platform had already plateaued.
In eight years of use I wasn't motivated to upgrade the CPU, motherboard,
monitor, or GPU. If I hadn't been a Microsoft employee during part of this
time I wouldn't have upgraded the OS either.

What killed the PC for me wasn't the hardware wearing out, it was mission
change. Like most people I now use the web rather than the local PC for most
of my computer activity. Maintaining the local PC is pure overhead, and I've
found it's easier to maintain a Mac than a PC.

It was nostalgic to turn on the PC again and poke through the Vista UI. It
brought back some good memories. (And I discovered some cute pictures of my
kids that I'd forgotten about.)

Thank you trusty PC. You served me and my family well!
