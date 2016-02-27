---
layout: post
title: "Ubuntu 15.10 on the Gigabyte p34w v5"
description: ""
category:
tags: []
---
{% include JB/setup %}

A few weeks ago I picked up a Gigabyte p34w v5 from
[Excaliber PC](http://www.excaliberpc.com/) to replace my previous work laptop.
In my normal fashion, I tried to get Linux running on it as soon as possible.
Because the p34w v5 features an Intel Skylake i7-6700HQ processor, getting
Linux running on it was a bit more complicated than normal, but still quite
easy.

## Machine specifications

{:.table}
| Device | Specification | Note |
|--------|---------------|------|
| CPU | i7-6700HQ | (Skylake) |
| iGPU | Intel HD Graphics 530 | |
| dGPU | NVIDIA GTX 970m | 3 GB GDDR5 |
| RAM | 16 GB | |
| Screen | 14" 1080p | |
| HDD | 1 TB WD Blue (wd10jpvx) | 5400 RPM |
| SSD | 256 GB Samsung 850 EVO | User-installed |
| Battery | 61.25 Wh | |
| USB | 3x 3.0, 1x 3.1 (USB-C) | |
| LAN | Yes | 10/100/1000 |
| HDMI output | Yes | |
| VGA output | Yes | |
| Audio output | Yes | 4-pin stereo + mic |
| SD Card Slot | Yes | |

## Hardware support

{:.table}
| Device | Status | Notes |
|--------|--------|-------|
| CPU | Supported | Upgrade to 4.5-rc4 kernel quickly |
| iGPU | Supported | See above |
| dGPU | Supported | See notes below |
| Keyboard | Partial | Everything except bluetooth enable/disable |
| Touchpad | Supported | |
| USB 2.0 / 3.0 | Supported | |
| USB 3.1 / USB-C | Untested | ASMedia ASM1142 USB 3.1 Host Controller |
| Webcam | Supported | SunPlus Innovation 1bcf:2c6b (720p) |
| Audio | Supported | Speakers, microphone, 4-pin audio jack |
| Bluetooth | Supported | On/off from keyboard appears broken |
| Wifi 2.4 / 5.0 GHz | Supported | (at least to 180 Mbps) |
| VGA Output | Supported | Works with both Intel and NVIDIA GPU |
| HDMI Output | Busted | Works with NEITHER Intel NOR NVIDIA GPU |
| Shutdown | Partial | Works only when `nvidia` module is loaded |
| Suspend | Partial | Works only when `nvidia` module is loaded |

## Installation

Support for Skylake CPUs is not the greatest in the Linux 4.2 kernel that
is shipped with Ubuntu 15.10, thus there are a few tricks to getting it to
work correctly. Basically, run the installer with `nomodeset` and then
upgrade to the latest kernel as quickly as possible to avoid a c-state lockup
bug. Here are some detailed instructions:

Run the 15.10 installer. At the first menu (where you can choose how
to run the OS image), highlight either the "try Ubuntu" or "install Ubuntu"
option and press the `e` key to edit the command-line option.

Navigation down to the line that begins with `linux`. Insert the word
`nomodeset` into the line prior to the triple-hyphens like this:

```
...
linux ... ro quiet splash nomodeset ---
```

Then press F10 to boot. Complete the installation, reboot the machine,
insert the same text into the kernel line and boot into Ubuntu by pressing F10.

Next we will install the Linux 4.5 kernel which includes more support
for the Skylake CPU. To do this open a terminal and type in the following:


    cd Downloads
    wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.5-rc4-wily/linux-headers-4.5.0-040500rc4-generic_4.5.0-040500rc4.201602141731_amd64.deb
    wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.5-rc4-wily/linux-headers-4.5.0-040500rc4_4.5.0-040500rc4.201602141731_all.deb
    wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.5-rc4-wily/linux-image-4.5.0-040500rc4-generic_4.5.0-040500rc4.201602141731_amd64.deb
    sudo dpkg -i linux*.deb
    sudo reboot

*Note:* Since this article was written, I updated to 4.5-rc5 and things appear
slightly more stable than on 4.5-rc4. I suggest grabbing the latest release
candidate until 4.5 is finalized.

## NVIDIA Graphics and GPU computing capabilities

Visit the [NVIDIA developer center](https://developer.nvidia.com/cuda-downloads)
and download the latest CUDA driver. The `x86_64` DEB package for Ubuntu
works fine. You can grab the file directly from here:

[http://developer.download.nvidia.com/compute/cuda/7.5/Prod/local_installers/cuda-repo-ubuntu1504-7-5-local_7.5-18_amd64.deb](http://developer.download.nvidia.com/compute/cuda/7.5/Prod/local_installers/cuda-repo-ubuntu1504-7-5-local_7.5-18_amd64.deb)

After the file has downloaded, simply install it using:

    sudo dpkg -i cuda-repo-ubuntu1504-7-5-local_7.5-18_amd64.deb
    sudo reboot

It's that simple! You can then switch between the Intel (integrated) and NVIDIA
(discrete) GPUs using `nvidia-prime` as follows:

    sudo prime-select intel  # switch to the integrated GPU
    sudo prime-select nvidia # switch to the discrete GPU
    sudo service restart lightdm

## SSD Caching (via. bcache)

Follow the instructions on my
[latest bcache post]({% post_url 2016-02-17-installing-ubuntu-1510-with-bcache-support %})
as this greatly simplifies the installation process. Note that the new method
is done directly within the installer.


## Battery life

{:.table}
| Test name    | Wifi | Screen | Duration (hr) | Claimed Rate (W/hr) | Calculated Rate (W/hr) | Notes |
| Idle runtime - Windows 10               | On | 50% | 6.94 hr | N/A | 8.8 | N/A |
| Idle runtime - Ubuntu 15.10 (Linux 4.5) | On | 50% | 4.5 hr | 12 - 13 | 13.61 | Stock Configuration |
| Idle runtime - Ubuntu 15.10 (Linux 4.5) | On | 50% | 5.5 hr | 11 - 12 | 11.9  | Powertop Tweaked |

## Other

### Build Quality

In general the laptop is well constructed, feels strong, and looks quite nice
(especially for a business professional who need a high-end GPU). I do; however,
have several quibbles:

#### 1. *The Keyboard*

Be aware that the left super / Windows key is, in reality, wired to the right
super / Windows key. Thus some window managers (e.g. Gnome) that distinguish
between the two keys will need to be reconfigured. For Gnome in particular,
you will need to either use `gconf` or `gnome-tweak-tool` to change the
"Switch between overview and desktop" key in the Keyboard and Mouse tab
to be the `R_SUPER` key.

I would also like to note that although I love the machine and find the build
quality to be quite good the keyboard is disappointing.
They keys all feel loose, chatter like crazy when typing, and
have very bad ghosting problems. I honestly considered returning the laptop
immediately because the keyboard feels so cheap. After using the machine
for a month, I'm still disappointed at the use of a low-quality keyboard on
a $1,400 laptop.

#### 2. *The Speakers*

The speakers are, beyond any reasonable doubt, the most horrible speakers
I've ever heard in a laptop. The speakers are very small and thus their
ability to produce a wide range of frequencies is highly limited. I always
use external speakers or headphones when listening to music on this machine
as there is no hope for producing decent quality sound from the given hardware.

### Optimus / bumblebee

It is possible to get optimus running via. bumblebee, but this takes a lot of
effort. I have a separate blog post half-written about this topic. I'll
add a link to this post to describe the process later.
