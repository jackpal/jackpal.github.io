---
date: 2022-02-13
description: Using Tailscale to bootstrap remote access to a Tablo DVR.
tags: Tablo Tailscale Debian Superbowl
title: Tailscale and Tablo
---

My son's away at college, without a TV. He wanted to watch the Superbowl. We
realized that one way to do that would be for him to access our family's
[Tablo DVR](https://www.tablotv.com/) remotely. Tablo supports remote access,
but there's a catch: The client software has to be set up while the Tablo device 
and the client machine are on the same local network.

But my son was 1400 miles away.

This seemed like a good opportunity to experiment with a [Tailscale](https://tailscale.com/)
private network. And therein lies a tale.

<!--more-->

If, like me, you've been [following the Tailscale company on Twitter](https://twitter.com/tailscale),
you probably expected me to report that it took only a few minutes to set up Tailscale,
and that everything went super smoothly. Unfortunately, while everything worked out
fine in the end, it took longer than the typical Tailscale success story.

The initial Tailscale enrollment went super smoothly, as everyone reports. But I soon
identified an issue: The Tablo DVR does not allow for third party software installation.
Therefore, in order to give my son access to it, I would need to configure a Tailscale
[subnet router](https://tailscale.com/kb/1019/subnets). And currently that feature
only works on a Linux host.

I don't currently run any Linux devices that are capable of installing Tailscale.
But I do have a Windows PC.

I decided to temporarily (just for the Superbowl) repurpose the Windows PC into a Linux box.

These day's I'm more comfortable with Mac / Linux than with Windows, so the following
uses the Mac for several steps where other people would probably use Windows.

To create a temporary Linux Tailscale subnet router, I did the following:

1. Downloaded a [Debian Live CD image](https://debian.org/CD/live/) to my Mac.
2. Used the [Etcher](https://www.balena.io/etcher/) app to create a bootable USB drive.
3. Reconfigured the PC's BIOS to boot from the USB drive.
4. Went through the Debian Live CD configuration process.
5. Speed-ran the Tailscale installation for a Linux box:

```bash
# The first two lines are specific to Debian bullseye, see Tailscale docs for other releases:
curl -fsSL https://pkgs.tailscale.com/stable/debian/bullseye.noarmor.gpg | sudo tee /usr/share/keyrings/tailscale-archive-keyring.gpg >/dev/null
curl -fsSL https://pkgs.tailscale.com/stable/debian/bullseye.tailscale-keyring.list | sudo tee

sudo apt-get update
sudo apt-get install tailscale

# I found I had to do the following, even though it's not documented.
sudo systemctl start tailscaled

sudo tailscale up

# Authenticate by using firefox to visit the URL that 'tailscale up' prints out.

# Enable port forwarding.

echo 'net.ipv4.ip_forward = 1' | sudo tee -a /etc/sysctl.conf
echo 'net.ipv6.conf.all.forwarding = 1' | sudo tee -a /etc/sysctl.conf
sudo sysctl -p /etc/sysctl.conf

sudo Tailscale up --advertise-exit-node --advertise-routes=192.168.86.0/24

# Then go to the Tailscale admin console and turn on the exit node and advertise routes switches for the newly added node.
```

Once this node was active, my son was able to access the Tablo by:

1. Joining his laptop to the family Tailscale network.
2. Setting the Debian node as his exit node.
3. Logging into the Tablo web site on his laptop.
4. Connecting his laptop to the family Tablo device.
   This step was the goal of this whole exercise.
   This is the step that Tailscale made possible.
5. Once he had connected once to the family Tablo device, he was able to disconnect from the Tailscale exit node.
6. At this point, his laptop was able to connect to the family Tablo device over the regular Internet.
7. At this point, I could turn off the Debian Tailscale node, and return the PC to its Windows configuration.

For what it's worth, once the Tablo app was set up, Tablo streaming worked better over the regular Internet than through the
Debian Tailscale subnet router. Presumably this was because of the overhead of routing packets through the Debian Tailscale subnet router.

# Conclusion

Things that went well:

+ The Tailscale free account creation process and signup process was excellent.
+ The Tailscale Mac and iOS apps were great.
+ The Tailscale documentation was great.
+ The Tailscale admin console was amazingly clear and responsive.
+ The Tailscale app error messages were excellent. They held my hand through the installation process.
  (Things like explaining that I needed to enable port forwarding, and giving the commands to use.)
+ The Debian Live CD recognized all the hardware necessary to connect to the Internet.
+ Once configured, the Tablo remote connect streaming software worked well.

Things that did not go so well:

- I had to install Linux just to run a Tailscale subnet router.
- The Tailscale Debian installation instructions omit the step of running `sudo systemctl start tailscaled` after installing Tailscale.

Overall my son and I give Tailscale two thumbs up! It enabled us to set up the Tablo DVR application remotely,
so that we could co-watch the Superbowl even though we were 1400 miles apart. Thanks Debian, Tablo, and Tailscale!
