---
layout: post
title: "Installing Ubuntu 12.10 on the HP Envy 6/6T"
description: ""
tagline: "Test"
category: 
tags: [HP Envy 6, HP Envy 6T, Linux, Ubuntu]
---
{% include JB/setup %}

As with my previous post on [installing Ubuntu 12.04](/blog/2012/09/02/installing-ubuntu-12-04-on-the-hp-envy-6/), the HP Envy 6 is not supported by Ubuntu 12.10 out of the box. Some work is required to get ATI switchable graphics working and the card reader still requires a manual driver installation. On the bright side, the drivers for the Broadcom BCM4313 \[14e4:4727\] wireless card work flawlessly out of the box and the kernel no longer requires  `acpi_backlight=vendor`.

As a change from my previous installation, I no longer dual boot Windows 7 and Ubuntu. If you install Windows 7 first and then install Ubuntu, it should detect the existing OS and configure Grub accordingly.

## What works?

* All primary components
** Processor, RAM, HDD, SSD, LAN (10/100/1000)
* UEFI (secure boot)
* ATI Switchable Graphics (see below)
* Video output (HDMI and VGA with HP adapter)
* Backlight/screen brightness buttons
* Wireless (802.11b/g tested)
* Webcam
* Integrated card reader (see below)
* Audio (speakers, built-in microphone, headphone jack)
* Input devices (keyboard: all hot keys, touchpad: no disable button)
* USB 2.0
* Bluetooth 2.0 (A2DP, mouse/keyboard input devices, etc.)
* AC / Battery state information
* Suspend and resume

## What doesn't work?

* Audio mute indicator light
* Disable touchpad button and light

## What wasn't tested?

* USB 3.0
* Wireless: 802.11n functionality
* Any special functions of the integrated Beats Audio system.

## ATI Hybrid Graphics (Radeon 7600m + Intel HD 3000)

Reading the [bug list](https://bugs.launchpad.net/fglrx/+bug/1068661/+index?comments=all) it is clear that this is a big problem. To summarize, at present only the open source drivers will work on hybrid systems. The drivers are ok, but slower than their closed-source counterparts. 

I suggest you follow the instructions on [Stack Overflow](http://askubuntu.com/questions/205112/ubuntu-12-10-amd-intel-hybrid-graphics-not-working). This will get OpenGL and OpenCL functioning.

Note, to get 3D acceleration when using the integrated GPU, you MUST edit `/etc/X11/Xsession.d/10fglrx` and append one of the following lines to the `LIBGL_DRIVERS_PATH` line:

32-bit systems:
    /usr/lib32/dri

64-bit systems:
    /usr/lib/x86_64-linux-gnu/dri

## The card reader

As with Ubuntu 12.04, the card reader does not work out of the box.  You simply need some UDEV rules (see [bug 971876](https://bugs.launchpad.net/ubuntu/+source/udisks/+bug/971876)) along with the driver.  Until this bug is patched and the driver merged into the upstream kernel, do this:

    wget http://planet76.com/drivers/realtek/rts-bpp-dkms_1.1_all.deb
    sudo apt-get install dkms
    sudo dpkg -i rts-bpp-dkms_1.1_all.deb
    echo 'DRIVERS=="rts_bpp", ENV{ID_DRIVE_FLASH_SD}="1"' | sudo tee -a /lib/udev/rules.d/81-udisks-realtek.rules

## The touchapd

As a note, right clicking is accomplished by a dual-finger tap. Dual finger scrolling can be enabled through the `Touchpad` tab in the `Mouse and Touchpad` settings.

## SSD performance and lifespan improvement

There are several performance tweaks you can apply to make the SSD run a little faster and last longer. The only one I'll suggest here is enabling [TRIM](http://en.wikipedia.org/wiki/TRIM) so garbage collection

To `/etc/fstab` add the `discard` option to your SSD device. For example:

    UUID=f0920b41-6ea1-404d-8d5e-5362624c77d5 /               ext4    discard,errors=remount-ro 0       1

## Next steps

One feature I really wish worked is the SSD caching feature (called 'Intel Smart Response Technology' in Windows). As far as I know nothing like this is ready for production use in Linux. Lets hope someone gets working on it soon!
