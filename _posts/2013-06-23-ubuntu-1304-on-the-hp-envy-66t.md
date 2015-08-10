---
layout: post
title: "Ubuntu 13.04 on the HP Envy 6/6T"
description: ""
category: 
tags: [Ubuntu, HP Envy 6, HP Envy 6T, Ubuntu 13.04, BCM4313]
---
{% include JB/setup %}

Continuing my posts on getting Ubuntu to run on the HP Envy 6/6T, we have a
(belated) update on getting Ubuntu 13.04 to run on this machine. My general
attitude during my [last post]() was quite good. In fact, nearly
everything worked right out of the box without much effort. The most difficult
thing was getting ATI switchable graphics to work which required some very
simple steps. In Ubuntu 13.04, things aren't as good. The wireless drivers
cause interference with other devices on the WLAN and getting ATI switchable
graphics to work requires a little more effort. On the bright side, everything
can be made to work!

One final note, I had a lot of issues upgrading from Ubuntu 12.10 to 13.04 
which I think were related to Gnome 3x configuration settings. I ended up
doing a completely clean install with the following partitioning configuration:

    /dev/sda3 fat32 ~105 MB partition from HP, don't touch
    /dev/sda5 swap 4 GB to match RAM
    /dev/sda6 ext4 /home rest of drive


    /dev/sdb1 efi - leave alone
    /dev/sdb2 ext2 /boot (a ~300 MB partition will keep 3-4 kernels)
    /dev/sdb3 ext4 /

Note that `/` and `/boot` are on the SSD to speed up most applications whereas
`/home` and `swap` are on the HDD.

## What works?

* All primary components
** Processor, RAM, HDD, SSD, LAN (10/100/1000)
* UEFI (secure boot)
* ATI Switchable Graphics (see below)
* Video output (HDMI and VGA with HP adapter)
* Backlight/screen brightness buttons
* Wireless (802.11b/g tested)
* Webcam
* Integrated card reader
* Input devices (keyboard: all hot keys, touchpad: no disable button)
* USB 2.0, USB 3.0
* Bluetooth 2.0 (A2DP, mouse/keyboard input devices, etc.)
* AC / Battery state information
* Suspend and resume

## What doesn't work (correctly)?

* Audio (crackling sounds due to scheduling issues)
* Audio mute indicator light
* Disable touchpad button and light
* Wireless enable/disable button (although the light works)

## What wasn't tested?

* Wireless: 802.11n functionality
* Any special functions of the integrated Beats Audio system.

## ATI Hybrid Graphics (Radeon 7600m + Intel HD 3000)

Like in my previous two block posts on this subject, ATI hybrid graphics
will not work out of the box. Last time things were fairly easy to get
working, now it is a little more complicated. Fortunately, all appears
to have been solved in [this AskUbuntu post](http://askubuntu.com/questions/205112/how-do-i-get-amd-intel-hybrid-graphics-drivers-to-work).
The steps are similar to last time, except you'll need to install a patched `libudev` and `xserver-xorg-video-intel` to not conflict with the ATI drivers. *Be sure to lock these packages after you install them so
they are not replaced by future upgrades!*

Note, the new `amdcce` (the ATI Control center GUI) doesn't call gksudo so you'll have to sudo yourself.

## Broadcom BCM4313 wireless

Under 12.10 wireless just worked. Now with the `bcmwl-kernel-source` v6.20.155.1 that ships with 13.04 it works, but it causes [heavy 
interference on the WLAN](https://bugs.launchpad.net/ubuntu/+source/bcmwl/+bug/1174145). At present the workaround is to upgrade the drivers to version 6.30.223.30-bdcom0-ubuntu1 from Ubuntu 14.04 (a.k.a. Ubuntu Saucy). You can download the new drivers from here [https://launchpad.net/ubuntu/saucy/+package/bcmwl-kernel-source](https://launchpad.net/ubuntu/saucy/+package/bcmwl-kernel-source).

## Audio issues

Sometimes you will hear a crackling sound, particularly whenever you
login/logout of Skype. This is a fairly easy to fix. Following the
[instructions on AskUbuntu](http://askubuntu.com/questions/157891/skype-and-vlc-sounds-sizzle-distorted-bad) simply add `tsched=0` to the `load-module module-udev-detect` line in `/etc/pulse/default.pa`

## Gnome shell

As with previous version of Ubuntu where Unity is used by default, if
the battery icon in Gnome shell is small/missing when you login, simply
install Ubuntu tweak and change the icon theme to `Humanity Dark` per
the [discussion on Launchpad](https://bugs.launchpad.net/light-themes/+bug/863663)


