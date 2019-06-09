---
layout: post
title: "Ubuntu 16.04 on the Gigabyte P34w v5"
description: ""
category: ["linux", "installation"]
tags: ["ubuntu", "Gigabyte p34w"]
---
{% include JB/setup %}

A few days ago I attempted to upgrade my Gigabyte P34w v5 from Ubuntu 15.10
(whose installation I documented
[in a previous post]({% post_url 2016-02-27-ubuntu-1510-on-the-gigabyte-p34w-v5 %})
) to Ubuntu 16.04 using the auto-upgrade functionality. Suffice it to say, that process
failed miserably due to some PPAs I forgot to disable. I salved the system, but something
really low level in the OS was busted so badly I was getting random segfaults from several
system-level binaries and couldn't load modules into the kernel. Thus, it was time to do
a fresh install of Ubuntu 16.04.

Suffice it to say support for the Gigabyte p34w v5 remains quite poor in the Ubuntu 16.04
installer. In particular, (1) the 4.4 kernel shipped with the installer has exceptionally
poor support for the Intel Skylake i7-6700HQ in the laptop, and (2) nouveau 1.0.12 does
not appear to function correctly with either the NVIDIA GTX 970m or optimus setup found
in the laptop. If you boot the installer and follow the standard installation method,
the machine will more than likely lock up (cores get stuck in a low-power state) or
you will encounter a slew of graphical issues once the installer is up and running.

A simple workaround for all of these problems is to simply install the server variant
of Ubuntu 16.04, upgrade the kernel from 
[the mainline kernel repository](http://kernel.ubuntu.com/~kernel-ppa/mainline), and
then install the desktop of your choice. However, I want to use my SSD to provide 
block-level caching (e.g. [bcache](http://kernel.ubuntu.com/~kernel-ppa/mainline)) 
of my my hard drive, thus I need access to a full shell during the installation process.

# Installation

The basic procedure is similar to my post on
[installing Ubuntu 15.10 on this laptop]({% post_url 2016-02-27-ubuntu-1510-on-the-gigabyte-p34w-v5 %}); 
however, this time we need additional kernel parameters.

1. Turn on the system and enter into the BIOS. Disable the discrete GPU by setting the
   XXXX graphic setting to "Disabled". Save and exit. Your machine will need to restart
   twice for the settings to apply.
2. Run the 16.04 installer. At the first menu (where you can choose how 
   to run the OS image), highlight either the "try Ubuntu" or "install Ubuntu"
   option and press the `e` key to edit the command-line option.
3. Navigate down to the line that begins with `linux`. Insert the text
   `intel_idle.max_cstate=0` into the line prior to the triple-hyphens like this:

    ```
    linux ... ro quiet splash intel_idle.max_cstate=0 ---
    ```

4. Press F10 to boot. Complete the installation, reboot the machine.
   (If you are going to install bcache, you do this before rebooting following the same
   instructions as 
   [found in my previous post on the topic]({% post_url 2016-02-17-installing-ubuntu-1510-with-bcache-support %})
   )
5. After the BIOS screen disappears, press ESC (or hold left shift, I'm not sure which
   works properly) to to get the GRUB boot menu.
6. Edit the default kernel line (e.g. the one selected) to add the same text as above.
   Continue booting Ubuntu to press F10.
7. Now download and install the latest 4.6 variant of the Linux Kernel from
   [the mainline kernel repository](http://kernel.ubuntu.com/~kernel-ppa/mainline).
   At the time of this writing, it is 4.6.2. You can do this using the following commands:

~~~~~
    cd Downloads
    wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.6.2-yakkety/linux-headers-4.6.2-040602-generic_4.6.2-040602.201606100516_amd64.deb
    wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.6.2-yakkety/linux-headers-4.6.2-040602_4.6.2-040602.201606100516_all.deb
    wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.6.2-yakkety/linux-image-4.6.2-040602-generic_4.6.2-040602.201606100516_amd64.deb
    sudo dpkg -i linux*.deb
    sudo reboot
~~~~~
  
*Note:* I am recommending the 4.6.x kernels rather than 4.7.x because NVIDIA's drivers packaged
with CUDA 7.5 (nvidia-352) and 8.0 (nvidia-361) do not build against 4.7.x kernels without
some tweaking yet.
8. After you have verified that your machine boots correctly, you can proceed with installing
   the graphics drivers. Don't enable the NVIDIA GPU until you have finished the NIVDIA driver
   installation!

## NVIDIA Graphics and GPU computing capabilities

Visit the [NVIDIA developer center](https://developer.nvidia.com/cuda-downloads)
and download the latest CUDA driver. For my installation I registered with the developer
network and downloaded the CUDA 8.0 release candidate. The CUDA 7.5 `x86_64` DEB package for 
Ubuntu 15.04 should be ok. If it doesn't build against 4.6, simply install Linux 4.5.7 or
later to get decent support of the CPU. You can grab the CUDA 7.5 DEB package directly from here:

[http://developer.download.nvidia.com/compute/cuda/7.5/Prod/local_installers/cuda-repo-ubuntu1504-7-5-local_7.5-18_amd64.deb](http://developer.download.nvidia.com/compute/cuda/7.5/Prod/local_installers/cuda-repo-ubuntu1504-7-5-local_7.5-18_amd64.deb)

After the file has downloaded, simply install it using:

    sudo dpkg -i cuda-repo-ubuntu1504-7-5-local_7.5-18_amd64.deb
    sudo reboot
    
Now re-enable the NVIDIA GPU in the BIOS. You can switch between the Intel (integrated) and NVIDIA
(discrete) GPUs using `nvidia-prime` as follows:

    sudo prime-select intel  # switch to the integrated GPU
    sudo prime-select nvidia # switch to the discrete GPU
    sudo service restart lightdm
    
## SSD Caching (via. bcache)

The process for enabling bcache support is identical to my previous post
[previous post for Ubuntu 15.10]({% post_url 2016-02-17-installing-ubuntu-1510-with-bcache-support %}). 
Just remember you need to create the bcache device prior to the partitioning step of the
installer.

## Post-install

### Keyboard

As before, the left super / Windows key is, in reality, wired to the right super / Windows key
so some window managers (e.g. Gnome) will need to be reconfigured to use the right super
key for the window selection function. Gnome, in particular, has this as an option in
`gnome-tweak-tool`

### Touchpad

Ubuntu 16.04 uses libinput for managing keyboards and mice. I found that the mouse acceleration
was really wacky on first boot so I had to change it using `xinput`. You can see and modify
settings using `xinput` as follows:

List the input devices on your computer:
    
    xinput --list --short

The touchpad is the "ETPS/2 Elantech Touchpad" device. You can set values by name, or
by the numeric ID. For example, to get a list of properties on my touchpad (device 15) I
can issue the following command:
    
    
    $ xinput --list-props 15
    Device 'ETPS/2 Elantech Touchpad':
	Device Enabled (137):	1
	Coordinate Transformation Matrix (139):	1.000000, 0.000000, 0.000000, 0.000000, 1.000000, 0.000000, 0.000000, 0.000000, 1.000000
	libinput Tapping Enabled (288):	1
	libinput Tapping Enabled Default (289):	0
	libinput Tapping Drag Enabled (290):	1
	libinput Tapping Drag Enabled Default (291):	1
	libinput Tapping Drag Lock Enabled (292):	0
	libinput Tapping Drag Lock Enabled Default (293):	0
	libinput Accel Speed (271):	0.389706
	libinput Accel Speed Default (272):	0.000000
	libinput Natural Scrolling Enabled (276):	0
	libinput Natural Scrolling Enabled Default (277):	0
	libinput Send Events Modes Available (255):	1, 1
	libinput Send Events Mode Enabled (256):	0, 0
	libinput Send Events Mode Enabled Default (257):	0, 0
	libinput Left Handed Enabled (278):	0
	libinput Left Handed Enabled Default (279):	0
	libinput Scroll Methods Available (280):	1, 1, 0
	libinput Scroll Method Enabled (281):	1, 0, 0
	libinput Scroll Method Enabled Default (282):	1, 0, 0
	libinput Disable While Typing Enabled (294):	1
	libinput Disable While Typing Enabled Default (295):	1
	Device Node (258):	"/dev/input/event9"
	Device Product ID (259):	2, 14
	libinput Drag Lock Buttons (287):	<no items>
	libinput Horizonal Scroll Enabled (260):	0
    
If you want to modify one of these settings, for example to disable horizontal scrolling
(property 260), you would issue a command as follows:

    xinput --set-prop 15 260 0
                             ^ disable horizontal scrolling
                         ^ property 260, horizontal scrolling found from above
                      ^ numeric device ID for my touchpad

### Optimus / bumblebee

*Update: on 2016 October 29*

As it turns out, the Gigabyte P34w v5 (and the P35 variant) both use a BIOS
that has a bug in its ACPI tables which cause
[several issues](https://github.com/Bumblebee-Project/Bumblebee/issues/764)
with bumblebee on this laptop. As of 2016 October 29, the bug has not been
fixed in a BIOS update, so we have to use the work around provided by user 
`Lekensteyn` in the previous link to change the OS reported to the BIOS
by the Linux kernel.

To do this, simply edit your `/etc/default/grub` configuration file using
your favorite editor and `sudo`. In the
`GRUB_CMDLINE_LINUX_DEFAULT` entry, add `acpi_osi=! acpi_osi=\"Windows 2009\""`
so that the entry appears like this:

```
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash acpi_osi=! acpi_osi=\"Windows 2009\""
```

After this, run `sudo update-grub`. After this I *think* bumblebee will function
like normal; however, I am still using the same procedure as I posted for
[bumblebee on the P34w v5 for Ubuntu 15.10]({% post_url 2016-03-09-optimusbumblebee-on-the-gigabyte-p34w-v5 %})
as it gives me a little more control over when the daemon is running.
Happy gaming!
