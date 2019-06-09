---
layout: post
title: "5x USB 2.0 ports on the WRTSL54GS"
description: "Modifying the WRTSL54GS to support 5x USB 2.0 ports"
category: ["hardware", "modding"]
tags: ["WRTSL54GS", "OpenWRT"]
---
{% include JB/setup %}

While doing a little spring cleaning today I found one of my old Linksys
WRTSL54GS router I purchased back in 2007. Like its better known sibling,
the WRT54G (after which projects like [OpenWRT](https://openwrt.org/) and
[DD-WRT](http://www.dd-wrt.com/) were named), this was a 5 port router with
2.4 GHz wireless networking. Most of the Linksys WRT-series routers were easily
modified with third-party firmware. Although it has been nearly a decade since
the first WRT-series router was released, they are still quite popular. Indeed,
the annoucement that a new generation of high-end WRT hardware, the
[WRT1900AC](https://www.linksys.com/en-us/press/releases/2014-01-06_Linksys_wrt_revolutionizes_wireless_networking)
came with much fanfare, but it appears Linksys has fallen short of their claims
of offering an open source firmware option when the product was released
(see [this thread](https://forum.openwrt.org/viewtopic.php?pid=230686#p230686) on
the OpenWRT forums). Furthermore, the device presently does not offer the high
level of performance one would expect for the $300 price tag (see the
[first](http://www.smallnetbuilder.com/wireless/wireless-reviews/32393-linksys-wrt1900ac-ac1900-dual-band-wireless-router-review) and
[second](http://www.smallnetbuilder.com/wireless/wireless-reviews/32400-linksys-wrt1900ac-review-more-tests)
review over at [Smallnetbuilder](http://www.smallnetbuilder.com).

Regardless of the present generation of hardware, seeing my old WRTSL54GS
caused feelings of nostalgia and reminded me of a long-lost blog post I never
wrote on my old websites: modifying the WRTSL54GS to support 5x USB 2.0 ports.
As it was sold in sorts, the WRTSL54GS came with a Broadcom system-on-chip
BCM4704KPB processor running at 266 Mhz, 8 MB of flash, 32 MB of RAM, a
Broadcom BCM4318 (BCM4318EKFBG) wireless controller and BCM5325 (BCM5325FKQM)
Ethernet controller. Unlike the first-generation WRT54G units that used VLAN
tagging to split a single switch into the WAN and LAN interfaces, the WRTSL54GS
had a dedicated WAN interface. Like most WRT-devices, it also had (unoccupied)
on-board headers for SERIAL, JTAG, and GPIO connections (see the
[OpenWRT wiki entry](http://wiki.openwrt.org/toh/linksys/wrtsl54gs) on this
device for more details). In addition to adding on pins for these ports, there
were several other modifications made including (successful) attempts to double
the RAM by
[soldering on a second 32 MB chip](http://www.linksysinfo.org/forums/showthread.php?t=46673)

## Installing a USB hub

What distinguished the WRTSL54GS from the 54G-series routers was that it had
"storage link" (hence the "SL" in the name) capabilities via. an onboard USB
1.0 / 2.0 controller. Shortly after people got their hands on the first units,
it was discovered that the engineers had built two USB headers on board, but
only connected one to an external jack:

<figure>
    <img class="img-responsive img-thumbnail"
        src="/images/blog/100_1937_1.jpg"
        alt="Second set of USB pins are below the existing USB port" />
    <figcaption>The second set of USB pins can be seen immediately below the
        installed USB port"</figcaption>
</figure>

I had already purchased a WRTSL54GS when I read about this find so I cracked
open my router to find out that there was plenty of room inside the case to
install a small USB hub:

<figure>
    <img class="img-responsive img-thumbnail"
        src="/images/blog/100_1924.jpg"
        alt="There is plenty of room for a USB hub at the top of the case" />
    <figcaption>There is plenty of room for a USB hub in the top of the case</figcaption>
</figure>

Fortunately there were some cheap, $12 USB 2.0 hubs for sale at our local Walmart
that had a fairly low profile, especially once cracked out of their case:

<figure>
    <img class="img-responsive img-thumbnail" style="float: left; width: 50%;"
       src="/images/blog/100_1929.jpg"
       alt="A cheap, low profile USB 2.0 hub" />
    <img class="img-responsive img-thumbnail" style="float: left; width: 50%;"
       src="/images/blog/100_1935.jpg"
       alt="Hub outside of its case" />
    <figcaption>A cheap low-profile USB hub</figcaption>
</figure>

After tracing out the ports I went to work on the plastic with my Dremel and
used some plastic epoxy to affix the USB hub to the case:

<figure>
    <img class="img-responsive img-thumbnail" style="float: left; width: 50%;"
        src="/images/blog/100_1959_1.jpg"
        alt="Three USB port openings cut nicely
            into the top of the WRTSL54GS case" />
    <img class="img-responsive img-thumbnail" style="float: left; width: 50%;"
        src="/images/blog/100_1965.jpg"
        alt="With the application of the epoxy, the
            hub wasn't going anywhere" />
    <figcaption>After cutting three holes into the top of the WRTSL54GS case
        and applying some epoxy, the hub wasn't going anywhere!</figcaption>
</figure>

Due to the port configuration of the hub, I now have four external USB 2.0
ports and one internal port which I used to expand non-volitile flash to 256 MB:

<figure>
    <img class="img-responsive img-thumbnail"
        src="/images/blog/100_1966.jpg"
        alt="The hub installed with a 256 MB flash drive in the internal slot" />
    <figcaption>The hub installed with a 256 MB flash drive in the internal slot</figcaption>
</figure>

## Getting the hub working

After flashing the router with OpenWRT, getting the additional USB ports working
was trivial. To enable USB 1.1 support we simply update opkg, install either the
UHCI or OHCI package, and load the modules


    opkg update
    opkg install kmod-usb-uhci
    insmod usbcore
    insmod uhci


(I forget if the router had UHCI or OHCI, but installing the OHCI modules is
similarly trivially accomplished: `opkg install kmod-usb-ohci` and
`insmod usb-ohci`). Next, you load the modules for USB 2.0:

    opkg update
    opkg install kmod-usb2
    insmod ehci-hcd

And that's it! After plugging in your USB drive you should see a message in
`dmesg` indicating the drive had been detected. You'll have to mount it to
somewhere useful using normal means (e.g. see `man mount`), but after that you
should have functional USB 2.0 storage.

## What use is USB on a router?

After modifying my router to have additional USB ports I started experimenting
with different applications for this new-found capability. First off I tried
network-based storage using SAMBA which worked, but was fairly slow. Much later,
when I used the router as a cheap, low-powered off-site backup solution, I used
`ssh` and the SSH filesystem (`sshfs`) and found this was adequate for backups
over the Internet.

Perhaps a more amusing application was sharing my USB HP printer using CUPS
and the [p910nd print server](http://wiki.openwrt.org/doc/howto/p910nd.server).
I was even able to get network-based scanning functioning using `sane` (see
a nice tutorial
[here](http://zed.0xff.me/2011/06/09/setting-up-a-scan-server-on-openwrt-and-netgear-wndr3700)
for how to configure it on a modern system.


