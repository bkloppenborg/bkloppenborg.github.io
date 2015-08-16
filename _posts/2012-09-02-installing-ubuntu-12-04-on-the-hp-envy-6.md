---
layout: post
title: "Installing Ubuntu 12.04 on the HP Envy 6"
description: ""
tagline: "Test"
category: linux
tags: [HP Envy 6, HP Envy 6T, Linux, Ubuntu]
---
{% include JB/setup %}

Shortly after starting my job at the Max Planck Institute for Radio Astronomy I received an HP Envy 6 to replace my older MSI MS1719 (GX700).  The HP Envy 6 is very similar to the HP Envy 6T so the instructions listed here should work for it as well.  I was able to get everything (including switchable graphics) working on Linux except the white "audio muted" LED on the keyboard and the "disable touchpad" button.

## What works?

* All primary components
** Processor, RAM, HDD, SSD, LAN (10/100/1000)
* ATI Switchable Graphics (see below)
* Backlight buttons (see below)
* Wireless (802.11b/g, see below for instructions)
* Webcam
* Integrated card reader (see below)
* Audio (speakers, built-in microphone, headphone jack)
* Input devices (keyboard, touchpad) including most hot keys (see below)
* USB 2.0
* Bluetooth 2.0 (A2DP, mouse/keyboard input devices, etc.)
* AC / Battery state information
* Suspend, hibernate, and resume

## What doesn't work?

* Audio mute indicator light
* Disable touchpad button

## What wasn't tested?

* USB 3.0
* Wireless: 802.11n functionality
* Any special functions of the integrated Beats Audio system.

## The details

Before we start talking about getting Linux running, lets discuss the specs.  The machine is an UltraBook class laptop so it is thin and very light.  For only $800 USD, I'd say the specs are quite reasonable:

* Size: 37.4 cm (14.72") W x 25.3 cm (9.95") D x 2 cm (0.78") H
* Processor: Intel Core i5-3317U CPU @ 1.70GHz (with VT support)
* Screen: 15.6" LED backlit at 1366x768 resolution
* Graphics: ATI Swichable graphics with an Intel HD 3000 and ATI Radeon 7600m
* RAM: 4 GB
* Storage: 32 GB Samsung SSD, 320 GB 5400 RPM Hitachi HDD
* Network: 10/100/1000 Ethernet
* Wireless: Broadcom BCM4313 802.11b/g/n
* OS: Windows 7 Home edition
* Other: Headphone out/microphone in, 2 x SuperSpeed USB 3.0, 1 x USB 2.0, HDMI port, Media Card Reader

Getting Ubuntu to run on the machine took a little bit of trial and error on my behalf, but hopefully this will benefit someone else.  I decided to keep a Windows partition on the machine (for gaming) so my first step was changing the way Windows is configured.  First I connected a USB DVD burner and wrote out the five Windows restore DVDs in case I messed something up.  Then I deleted the restore partition from the drive and created an empty extended partition into which Linux could be installed.  After this, I disabled the Intel Smart Caching (this uses the SSD as a cache for frequently accessed files from the hard drive and leads to a SIGNIFICANT performance increase).  This freed up 28/32 GB on the SSD.  (The remaining 4GB is used by Windows's resume functions and I haven't figured out how to move it, but for now this is ok.)

## Installing Ubuntu

Next I started the Ubuntu installer from a USB disk.  To boot add the `acpi_backlight=vendor` option to the kernel.  Because Intel Smart Caching isn't supported in Linux, I put the main OS (i.e. "/") onto the SSD and "/home" onto the HDD.  My choice in doing it this was was motivated by reading this and from some other benchmarking article (whose URL I have forgotten) which indicated you get the best performance by putting the OS files on the SSD.

In retrospect, I think I should have placed /tmp, /var/log and a few other frequently-written-to locations on the HDD (or, in the case of /tmp, in a RAM disk) to save the SSD a few thousand writes a day.
Display and backlight

Once you get Ubuntu installed, the display probably won't work correctly.  If (when?) this happens, reboot into recovery mode and drop to a console.  Then edit the GRUB2 boot parameters in `/etc/default/grub` to add `acpi_backlight=vendor` to the kernel parameters , as follows:

    GRUB_CMDLINE_LINUX_DEFAULT="splash acpi_backlight=vendor"
    GRUB_CMDLINE_LINUX="acpi_backlight=vendor"

Then update grup with a `sudo update-grub` and reboot.

Note, I'm not sure if these changes will persist after a kernel update, so keep them in mind.

# Broadcom BCM4313 \[14e4:4727\] wireless card drivers

Getting wireless working took me quite a while to figure out.  By default the brcmsmac drivers included with the kernel will be loaded.  These work, but the signal strength I received from my access point was very, very bad.  Installing the broadcom-kernel-source package (which at the time of this writing has version 5.100.82.38) increased the signal strength considerably, but took forever to connect and tended to have horrible ping times to my router (ranging from 1ms to 9000 ms).

The lengthy connection time issue is addressed by a nice post by pof.  He even includes a PPA with the updated 5.100.82.112 drivers.  So, I took his PPA version to get the updated driver:

    sudo apt-get purge bcmwl-kernel-source
    sudo add-apt-repository ppa:poliva/pof
    sudo apt-get update
    sudo apt-get install broadcom-sta-dkms

My problem with ping times was related to power management.  As explained on StackOverflow, all you need to do is

    gksu gedit /etc/pm/config.d/blacklist

and put the line

    HOOK_BLACKLIST="wireless"

in it.  With these two changes, the hardware switch, indicator light, and power management should function normally.  If you still have problems with ping times, try manually disabling power management with

    sudo iwconfig wlan0 power off

and reconnect to the AP.  If this fixed the issue, see the post by jrg at the bottom of the aforementioned StackOverflow page.

I would really suggest trying some different power settings to try to optimize power settings so battery life might be extended.

## ATI Switchable Graphics

This was surprisingly easy to get working correctly.  Unfortunately switchable graphics are not as clean as they are in Windows (namely automatic).  In order to switch between GPUs you need to run the ATI configuration utility (either command line or the GUI) and select the other graphic device.  Then, restart X (i.e. logout then login again).

Installing the drivers is quite easy.  If you want to use the drivers from Ubuntu, just do this:

1. Install ATI drivers from the restricted drivers manager
2. Before rebooting, type the following into a terminal to rewrite the X11 configuration file:
    sudo aticonfig --initial -f
3. Restart

At this point, the ATI graphics will work, but the Intel HD 3000 graphics will lack any hardware acceleration.  To fix this follow step 2 instructions [here](http://ubuntuforums.org/showthread.php?t=1930450) paying close attention to the 32 or 64-bit instructions.

If you require the latest drivers from ATI, purge all ATI graphics-related packages from your system then install the drivers from ATI.  Repeat everything after step 2 above.

Doing this I have switchable graphics working nicely.  I've confirmed that OpenGL, OpenCL (GPU computing library), and HDMI out work when using the discrete (ATI) card.  OpenGL and HDMI out when using Intel Graphics.

## The card reader

The card reader does not work out of the box.  You simply need some UDEV rules (see bug 971876).  Until this bug is patched, do this:

    wget http://planet76.com/drivers/realtek/rts-bpp-dkms_1.1_all.deb
    sudo apt-get install dkms
    sudo dpkg -i rts-bpp-dkms_1.1_all.deb
    echo 'DRIVERS=="rts_bpp", ENV{ID_DRIVE_FLASH_SD}="1"' | sudo tee -a /lib/udev/rules.d/81-udisks-realtek.rules

#A note about the touchpad

Left click by either a single finger tap, or by depressing the lower portion of the touchpad.
Right click is accomplished by two-finger tapping.  Depressing the lower-right side of the touchpad is treated as a left click.
Disable touchpad button not working

# Battery life

With WIFI power management disabled and using the ATI card, I've consistently seen four hours of battery life.  Using the Intel HD 3000 graphics I know it lasts longer (perhaps five hours total), but I haven't extensively timed it.
