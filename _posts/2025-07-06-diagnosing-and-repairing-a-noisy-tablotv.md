---
layout: post
title: "Fixing My Noisy TabloTV"
date: 2025-07-06 13:00:00
description: "A step-by-step guide to diagnosing and fixing a faulty TabloTV, from repairing the filesystem to replacing the hard drive and power adapter."
tags: tablotv chirping coil_whine repair fsck ddrescue ssd
---

A few weeks ago, my [TabloTV](https://support.tablotv.com/hc/en-us/articles/202083158-Legacy-2-Tuner-and-4-Tuner-Tablo-Quick-Start-Guide), which I use to record over-the-air TV, started making strange noises. I could hear the hard drive seeking constantly and a separate chirping sound. Eventually, it failed completely. The UI showed a "No Storage" error, and I couldn't watch any of my recordings.

Here’s how I diagnosed and fixed the problems.

### Problem 1: The "No Storage" Error

My first goal was to get my recordings back. TabloTV's support suggested I replace the hard drive, but I wanted to see if I could repair it first. I learned from the TabloTV forums that the device uses the `ext4` filesystem, and that other people had been able to fix this issue by attaching their TabloTV's drive to a Linux PC and using the `fsck` command to repair the filesystem.

I don't have a general-purpose Linux PC at home. But I do have a Mac. I used [UTM](https://github.com/utmapp/UTM) to run an Ubuntu Server virtual machine on my Mac. I enabled USB sharing in UTM, which let the VM see the TabloTV's hard drive when I plugged it in.

Inside the VM, the drive appeared as `/dev/sdb`. I ran `fsck` to repair it:

```bash
sudo fsck /dev/sdb1
```

I put the drive back into the TabloTV, and it worked. The "No Storage" error was gone, and my recordings were accessible again. However, the drive was still making constant seeking noises.

### Problem 2: Replacing the Hard Drive

It wasn't clear whether the drive was failing or not. It seemed fine when accessed from the Linux VM. To rule out the possibility of a failing drive, I decided to replace the old 2TB spinning drive with a new 2TB SSD. To save my recordings, I needed to clone the old drive to the new one.

I first tried using `rsync` inside my Ubuntu VM to copy the files, but it was too slow, running at only 2.5 MB/s.

A full disk clone is much faster. I switched to a tool called `ddrescue`. To get the best performance, I installed `ddrescue` with Homebrew and ran it directly on my Mac, avoiding the VM overhead.

I used `diskutil list` to find the drive names. For my setup, the old drive was `disk20` and the new SSD was `disk21`. I ran this command to clone the drive:

```bash
sudo ddrescue -f -v -c 262144 /dev/rdisk20 /dev/rdisk21 ddrescue.logfile
```

The cloning process took 36 hours. It completed with no errors. I installed the new SSD in the TabloTV. The seeking noises were gone, and the TabloTV's UI felt faster.

### Problem 3: The Chirping Noise

Even with the new, silent SSD, the TabloTV unit itself still made a high-pitched chirping sound.

I went back to the forums and found that a faulty power adapter can cause this. The original adapter was a 12V 2A model. I found a spare power adapter with the same specs from an old cable modem and swapped it in.

That fixed it. The chirping noise was  gone.

### Conclusion

My TabloTV had two problems: a corrupted filesystem and a bad power adapter. I fixed the filesystem with `fsck` and the noise by swapping the power adapter. I probably didn't need to replace the hard drive, but the new SSD made the TabloTV silent and faster.
