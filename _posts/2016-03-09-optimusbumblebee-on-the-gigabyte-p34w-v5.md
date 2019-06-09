---
layout: post
title: "Optimus on the Gigabyte p34w v5 under Ubuntu 15.10"
description: ""
category: ["linux", "installation"]
tags: ["ubuntu", "Gigabyte p34w"]
---
{% include JB/setup %}

Through much experimentation I have *finally* been able to get optimus to
work on the Gigabyte p34w v5; however, the specific implementation is far from
flawless. In particular, starting the bumblebee daemon on boot causes the system
to hang indefinitely. Thus one must start and stop `bumblebeed` as needed.

Before getting started, please read through my
[previous post about this machine]({% post_url 2016-02-27-ubuntu-1510-on-the-gigabyte-p34w-v5 %})
and ensure you are running the linux 4.5-rc4 or later kernel.

## Installing software

Getting bumblebee installed is a bit interesting on 15.10. The only instructions
I found that work can be
[found in this StackOverflow post](http://askubuntu.com/questions/689924/bumblebee-intelnvidia-on-15-10-blackscreen-issue).
Because `bumblebeed` causes the machine to hang on boot, I've had to modify
the instructions somewhat.

1. Boot your machine
2. Install `nvidia` driver and bumblebee in the same command. I actually used
   the CUDA Debian package for Ubuntu 15.04 described in my
   [previous post about the p35w v5]({% post_url 2016-02-27-ubuntu-1510-on-the-gigabyte-p34w-v5 %})
   however you can use anything after `nvidia-352` where support for the GTX 970m
   was added. The Simple method of doing this is as follows:

    sudo aptitude install bumblebee nvidia-352 prime-select

3. After this is done, *do not reboot*. Instead immediately switch to the
   intel driver using `prime-select`:

    sudo prime-select intel

4. Now configure bumblebee. Edit `/etc/bumblebee/bumblebee.conf` as
   follows: Set `Driver=nvidia` and replace `nvidia-common` with `nvidia-352` in the
   `[driver-nvidia]` section. I've posted my full `bumblebeed.conf` file below
   as a reference if you need it.

5. Next `/etc/bumblebee/xorg.conf.nvidia`, uncomment `BusID "PCI:01:00:0"`

7. Now disable the bumblebeed service with the following command

    sudo systemctl disable bumblebeed.service

8. Reboot the system. With any luck it will come up running on the Intel card.

## Using Optimus / Bumblebee

Because we disabled the bumblebee daemon on boot, we have to activate it whenever
we want to use it. This is fairly easy to do with a few scripts.
To turn on the NVIDIA card and start `bumblebeed`, I execute this
`nvidia-start.sh` script:

    #!/bin/bash

    sudo modprobe bbswitch
    sudo service bumblebeed start

And to put the machine back into a power-saving mode after using the
NVIDIA card, I use the following `nvidia-stop.sh` script:

    #!/bin/bash

    sudo modprobe bbswitch
    sudo rmmod nvidia_uvm
    sudo rmmod nvidia
    sudo service bumblebeed stop
    sudo tee /proc/acpi/bbswitch <<< OFF
    cat /proc/acpi/bbswitch

The `nvidia-stop.sh` script (1) unloads all of the modules (including the NVIDIA
unified virtual memory module if you use CUDA), (2) shuts down the bumblebee
service, (3) tells bbswitch to shut off the card, and then (4) echos the
current state of the card.

## Power consumption

Using a [Kill A Watt](http://www.p3international.com/products/p4400.html)
power usage meter I checked the power consumption of my laptop with and without
the NVIDIA card enabled. My results, shown in the following table, show that
the bumblebee and bbswitch do indeed disable the discrete GPU; however, there
appears to be a small (0.4 Watt) draw when the card is off, but not disabled.

{:.table}
| Idle State | Consumption (W) | Note |
|------------|-----------------|------|
| Integrated only   | 14.5 | Discrete GPU disabled in BIOS |
| bbswitch off      | 14.9 | |
| bbswitch on       | 30.4 | |

## Running applications on the discrete card

To execute an application on the NVIDIA card, simply run it using either
`optirun` or `primusrun` as follows:

    optirun program-name

One thing you will undoubtably notice is that your FPS is inherently limited
to about 60, which is the default vsync rate of the intel graphics card.
To get higher throughput, simply change the vblank mode. Here is an example
of how you could run glxgears

    export vblank_mode=0
    optirun glxgears
    export vblank_mode=1

Lastly, here are a few performance results for the classic FPS, Nexuiz:

{:.table}
| Driver | Program | Bridge | Transport Method | PMMethod | FPS |
|--------|---------|--------|------------------|----------|----:|
| i915   | N/A     | N/A    | N/A              | N/A      | 184 |
| nvidia | optirun | primus | ---              | N/A      | 257 |

As you can see, we get a 40% boost in performance by running Nexuiz via.
optirun. I suspect the difference would be more considerable for more modern
games that push the GPU's capabilities.

After getting it working, I haven't experimented with different bridge methods
(valid options are `auto`, `virtualgl` and `primus`) or with transport methods
for VirtualGL (the options are `proxy`, `jpeg`, `rgb`, `xv`, and `yuv`) although
I hear the performance differences are not considerable.

## Configuration files

In case you need them, here are several of my configuration files:

My `/etc/bumblebee/bumblebee.conf` file:

    [bumblebeed]
    VirtualDisplay=:8
    KeepUnusedXServer=false
    ServerGroup=bumblebee
    TurnCardOffAtExit=true
    NoEcoModeOverride=false
    Driver=nvidia
    XorgConfDir=/etc/bumblebee/xorg.conf.d

    [optirun]
    Bridge=primus
    VGLTransport=proxy
    PrimusLibraryPath=/usr/lib/x86_64-linux-gnu/primus:/usr/lib/i386-linux-gnu/primus
    AllowFallbackToIGC=false

    [driver-nvidia]
    KernelDriver=nvidia-352
    PMMethod=auto
    LibraryPath=/usr/lib/nvidia-352:/usr/lib32/nvidia-352
    XorgModulePath=/usr/lib/nvidia-352/xorg,/usr/lib/xorg/modules
    XorgConfFile=/etc/bumblebee/xorg.conf.nvidia

    [driver-nouveau]
    KernelDriver=nouveau
    PMMethod=auto
    XorgConfFile=/etc/bumblebee/xorg.conf.nouveau

And the `/etc/bumblebee/xorg.conf.nvidia` file:

    Section "ServerLayout"
        Identifier  "Layout0"
        Option      "AutoAddDevices" "false"
        Option      "AutoAddGPU" "false"
    EndSection

    Section "Device"
        Identifier  "DiscreteNvidia"
        Driver      "nvidia"
        VendorName  "NVIDIA Corporation"

        # This should be the only line you need to modify by default.
        # just uncomment it.
        BusID "PCI:01:00:0"

        Option "ProbeAllGpus" "false"

        Option "NoLogo" "true"
        Option "UseEDID" "false"
        Option "UseDisplayDevice" "none"
    EndSection

## Ancillary information

### How to run nvidia-settings

After bumblebee is running, simply execute `nvidia-settings` using optirun on
the same virtual display specified in the `/etc/bumblebee/bumblebee.conf` file:

    optirun nvidia-settings -c :8

This opens nvidia-settings on virtual terminal 8, where the NVIDIA GPU is
rendering frames.

### Check or change the power state for the discrete GPU

The power state of the discrete GPU is managed by
[bbswitch](https://github.com/Bumblebee-Project/bbswitch).
After the module is loaded, you can manually change the power state of the
discrete GPU using the following commands:

    sudo tee /proc/acpi/bbswitch <<<OFF
    sudo tee /proc/acpi/bbswitch <<<ON

Please note that the discrete GPU cannot be in use when you attempt to turn
it off, hence you might need to unload the `nvidia` and `nvidia-uvm` kernel
modules (see the scripts above) for examples.


