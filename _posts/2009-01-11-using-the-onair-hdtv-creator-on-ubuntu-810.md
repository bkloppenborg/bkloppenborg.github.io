---
layout: post
title: "Using the OnAir HDTV Creator on Ubuntu 8.10"
description: "How to setup and use the OnAir HDTV creator on Ubuntu Linux 8.10"
category: ["hardware", "old-content"]
tags: ["ubuntu", "onair creator", "autumnwave", "hdtv"]
---
{% include JB/setup %}

This blog post was originally a comment on the V4L wiki for the AutumnWave Onair USB HDTV Creator; however, I wasn't easily able to locate it last time I needed the information. On 2012-11-18 I moved it here.

-----

The [OnAir USB HDTV Creator](http://www.autumnwave.com/index.php/products/tv-tuners/onair-creator) is dual purpose Digital/Analog TV tuner with hardware-based MPEG2 encoding for analog (RCA, S-Video and NTSC/PAL inputs).  Support for this device was added in the Linux kernel [v 2.6.27](http://www.kernel.org/pub/linux/kernel/v2.6/ChangeLog-2.6.26).

*Disclaimer:* I am running kernel 2.6.27-9, Ubuntu Linux 8.10 x86_64. Instructions for other systems should be similar.

A change in the 2.6.27 kernel broke a portion of the pvrusb2 module.  Until this issue is resolved, it is necessary to load the module manually before connecting the tuner to your computer:

    $ sudo modprobe pvrusb2 initusbreset=0

This will probably be fixed in later versions of the kernel. Now, plug in the tuner and type

    $ dmesg

you should see a series of lines that read something similar to the following:

    pvrusb2: Hauppauge WinTV-PVR-USB2 MPEG2 Encoder/Tuner : V4L in-tree version
    pvrusb2: Debug mask is 31 (0x1f)
    usb 1-1: new high speed USB device using ehci_hcd and address 2
    usb 1-1: configuration #1 chosen from 1 choice
    saa7115' 3-0021: saa7115 found (1f7115d0e100000) @ 0x42 (pvrusb2_a)
    tuner' 3-0043: chip found @ 0x86 (pvrusb2_a)
    tda9887 3-0043: creating new instance
    tda9887 3-0043: tda988[5/6/7] found
    tuner' 3-0061: chip found @ 0xc2 (pvrusb2_a)
    cs53l32a' 3-0011: chip found @ 0x22 (pvrusb2_a)
    pvrusb2: Supported video standard(s) reported available in hardware: PAL-B/B1/D/D1/G/H/I/K/M/N/Nc/60;NTSC-M/
    pvrusb2: Mapping standards mask=0xffffff (PAL-B/B1/D/D1/G/H/I/K/M/N/Nc/60;NTSC-M/Mj/443/Mk;SECAM-B/D/G/H/K/K1/L/LC)
    pvrusb2: Setting up 28 unique standard(s)
    pvrusb2: Set up standard idx=0 name=PAL-B/G
    pvrusb2: Set up standard idx=1 name=PAL-D/K
    pvrusb2: Set up standard idx=2 name=SECAM-B/G
    pvrusb2: Set up standard idx=3 name=SECAM-D/K
    pvrusb2: Set up standard idx=4 name=PAL-B
    pvrusb2: Set up standard idx=5 name=PAL-B1
    pvrusb2: Set up standard idx=6 name=PAL-G
    pvrusb2: Set up standard idx=7 name=PAL-H
    pvrusb2: Set up standard idx=8 name=PAL-I
    pvrusb2: Set up standard idx=9 name=PAL-D
    pvrusb2: Set up standard idx=10 name=PAL-D1
    pvrusb2: Set up standard idx=11 name=PAL-K
    pvrusb2: Set up standard idx=12 name=PAL-M
    pvrusb2: Set up standard idx=13 name=PAL-N
    pvrusb2: Set up standard idx=14 name=PAL-Nc
    pvrusb2: Set up standard idx=15 name=PAL-60
    pvrusb2: Set up standard idx=16 name=NTSC-M
    pvrusb2: Set up standard idx=17 name=NTSC-Mj
    pvrusb2: Set up standard idx=18 name=NTSC-443
    pvrusb2: Set up standard idx=19 name=NTSC-Mk
    pvrusb2: Set up standard idx=20 name=SECAM-B
    pvrusb2: Set up standard idx=21 name=SECAM-D
    pvrusb2: Set up standard idx=22 name=SECAM-G
    pvrusb2: Set up standard idx=23 name=SECAM-H
    pvrusb2: Set up standard idx=24 name=SECAM-K
    pvrusb2: Set up standard idx=25 name=SECAM-K1
    pvrusb2: Set up standard idx=26 name=SECAM-L
    pvrusb2: Set up standard idx=27 name=SECAM-LC
    pvrusb2: Initial video standard (determined by device type): NTSC-M
    pvrusb2: Device initialization completed successfully.
    pvrusb2: registered device video0 [mpeg]
    DVB: registering new adapter (pvrusb2-dvb)
    tuner-simple 3-0061: creating new instance
    tuner-simple 3-0061: type set to 64 (LG TDVS-H06xF)
    saa7115' 3-0021: Audio frequency: 48000 Hz
    saa7115' 3-0021: Input:           Composite 4
    saa7115' 3-0021: Video signal:    bad
    saa7115' 3-0021: Frequency:       50 Hz
    saa7115' 3-0021: Detected format: BW/No color
    saa7115' 3-0021: Width, Height:   720, 480
    tda9887 3-0043: Data bytes: b=0xd4 c=0x30 e=0x44
    tuner' 3-0061: Tuner mode:      analog TV
    tuner' 3-0061: Frequency:       175.25 MHz
    tuner' 3-0061: Standard:        0x00001000
    cs53l32a' 3-0011: Input:  2
    cs53l32a' 3-0011: Volume: 0 dB
    firmware: requesting v4l-cx2341x-enc.fw
    DVB: registering frontend 0 (LG Electronics LGDT3303 VSB/QAM Frontend)...
    tuner-simple 3-0061: attaching existing instance
    tuner-simple 3-0061: type set to 64 (LG TDVS-H06xF)

*Notice:* that the analog portion of the driver is using firmware for Hauppauge WinTV-PVR-USB2 MPEG2 Encoder/Tuner. I have yet to test the analog tuning features of the OnAir HDTV Creator.

If you intend to use the tuner on a regular basis and are stuck with the 2.6.27 kernel, you may wish to add an entry anywhere in a file in `/etc/modprobe.d/`

    options pvrusb2 initusbreset=0

which automatically loads the module with the specified parameter.

Now, if you plan on viewing ATSC/QAM channels, you will need to scan for them.  Full details on the following section are found here in the manpages for [scan](http://www.linuxtv.org/wiki/index.php/Scan).  The scanning program simply sets the frequency of the DTV tuner and then looks for signal.  Because of the wide array of frequencies used in different countries, in order to scan you need a scan file that contains the frequencies used in your country.  Common locations for these files are:

    /usr/share/dvb/
    /usr/share/doc/dvb-utils/examples/scan/
    /usr/share/doc/dvb-apps/
    /usr/local/share/dvb/scan/

On Ubuntu 8.10, the default scan files are contained in `/usr/share/doc/dvb-utils/examples/scan/dvb-t/`.  After you find the location of the files, browse through the subdirectories to find the scan file that applies to your location.  In the case of US ATSC, the file is located in `/usr/share/doc/dvb-utils/examples/scan/atsc`.

With the scan files, you are now ready to scan for stations.  On Ubuntu 8.10, the DTV scanning program is called `scan`, but it may be called `dvbscan` on other systems.  To scan for US ATSC channels:

    $ scan /usr/share/doc/dvb-utils/examples/scan/atsc/us-ATSC-center-frequencies-8VSB

Similarly, for QAM encrypted cable, you can use

    $ scan /usr/share/doc/dvb-utils/examples/scan/atsc/us-Cable-Standard-center-frequencies-QAM256 

Now, we need to store the output from the scan file for use by other programs.  If you wish to receive ATSC OTA stations, you would use the .azap directory:

    $ mkdir ~/.azap
    $ scan /usr/share/doc/dvb-utils/examples/scan/atsc/us-ATSC-center-frequencies-8VSB > ~/.azap/channels.conf

For countries that used dvb-c, dvb-s, or dvb-t, refer to the [scan manpage](http://www.linuxtv.org/wiki/index.php/Scan) for more information.  While scan is running, you will see output on your screen similar to the following:

    scanning /usr/share/doc/dvb-utils/examples/scan/atsc/us-ATSC-center-frequencies-8VSB
    using '/dev/dvb/adapter0/frontend0' and '/dev/dvb/adapter0/demux0'
    >>> tune to: 57028615:8VSB
    WARNING: >>> tuning failed!!!
    >>> tune to: 57028615:8VSB (tuning failed)

    ...

    >>> tune to: 485028615:8VSB
    service is running. Channel number: 9:1. Name: 'KUSA-HD'
    service is running. Channel number: 9:2. Name: 'WX-Plus'
    >>> tune to: 491028615:8VSB
    service is running. Channel number: 7:1. Name: 'KMGH-HD'
    service is running. Channel number: 7:27. Name: 'KZCO-SD'
    >>> tune to: 497028615:8VSB
    WARNING: filter timeout pid 0x0000
    WARNING: filter timeout pid 0x1ffb

Clearly, stations that work are prefixed by "service is running."  Stations that have poor reception or have other technical difficulties often time-out. For me, my `channels.conf` file looks like this:

    KUSA-HD:485028615:8VSB:49:52:1
    WX-Plus:485028615:8VSB:65:68:2
    KMGH-HD:491028615:8VSB:49:52:3
    KZCO-SD:491028615:8VSB:65:68:4

    ...

    KCEC-DT:695028615:8VSB:49:52:3

The text at the beginning is a user-defined string (with no spaces) that will be used to identify the station later.  If you have gotten this far, your tuner is working properly.

Now, the LinuxTV Wiki suggests using a program called zap to test your tuner (see [zap](http://linuxtv.org/wiki/index.php/Dvbscan)).  For ATSC, we use azap.  If I wished to tune KUSA-HD from the above list, I open a shell and type:

    $ azap -r 'KUSA-HD'

For which I receive

    using '/dev/dvb/adapter0/frontend0' and '/dev/dvb/adapter0/demux0'
    tuning to 485028615 Hz
    video pid 0x0031, audio pid 0x0034
    status 00 | signal 0000 | snr 0000 | ber 00000000 | unc 00000000 | 
    status 1f | signal a791 | snr 16e8 | ber 00000000 | unc 00000000 | FE_HAS_LOCK
    status 1f | signal b54e | snr 18c9 | ber 00000000 | unc 0000003d | FE_HAS_LOCK
    status 1f | signal bbee | snr 19b1 | ber 00000000 | unc 00000000 | FE_HAS_LOCK

Full documentation on the meaning of these columns is found on the LinuxTV Wiki.  The important columns are the status column (00 means the tuner is initialized, but nothing has been decoded, 1f means tuning is working) and the last column (FE_HAS_LOCK means everything is working).  Sadly, I was not able to get any of the suggested testing methods to work (see [Testing your DBV device](http://linuxtv.org/wiki/index.php/Testing_your_DVB_device)), but I did find that xine works with ATSC stations.

To watch DTV, install xine (on Ubuntu, sudo apt-get install xine-ui) run it to create the configuration directory, and then copy the channels.conf file over to the .xine directory:

    $ cp ~/.azap/channels.conf ~/.xine/

Now, launch xine and click on the DTV button.  Using the playlist, you can change between stations.

