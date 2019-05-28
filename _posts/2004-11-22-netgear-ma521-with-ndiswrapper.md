---
layout: post
title: "Configuring the Netgear MA521 with ndiswrapper in Linux"
date: 2004-11-12T00:00:01-06:00
draft: false
author: "Brian Kloppenborg"
tags: ["ndiswrapper"]
categories: ["linux", "wifi"]
---

Although the slew of Linux commands in the following section might seem like 
they are a bit cumbersom, it is actually not that difficult to configure the 
MA521 to use ndiswrapper.

We first start with the basic install of ndiswrapper. Follow the instructions 
in the ndiswrapper readme. You must be superusered in order to install ndiswrapper. 
This can be accomplished from a user-shell by typing _su_ and then supplying 
your root password. If I recall the installation instructions, you type 

     ./configure
    make
    make install
    
Now we install the drivers for the MA521. I have found that the drivers provided 
by netgear make my distribution (Suse 9.0 pro) very unstable. I suggest downloading 
the driver from [RealTek](ftp://210.51.181.211/cn/wlan/rtl8180l/ndis5x-8180(170).zip ). 
I currently use these drivers and they work fine. Follow the installation instructions 
from ndiswrapper on how to install a driver into ndiswrapper.

Now, we load the driver into the kernel. As root:

    modprobe ndiswrapper_

Now, we use iwconfig to setup the card to the access point that we 
 wish. By typing
 
    iwconfig

One should see the list of devices that are part of your system. wlan0 should 
be your MA521 wireless card. I have provided a table of some common configuration 
options for iwconfig below along with command structures:

## Configuring to access an AP without WEP

Type the following at the root prompt:

    iwconfig wlan0 mode managed
    iwconfig wlan0 channel CHANNEL_NUMBER
    iwconfig wlan0 essid SSID_NAME
    
Then bring up the interface as described below

## Configuring to access a AP with WEP

Type the following at the root prompt:

    iwconfig wlan0 mode managed
    iwconfig wlan0 channel CHANNEL_NUMBER
    iwconfig wlan0 essid SSID_NAME
    iwconfig wlan0 key WEP_KEY
    
Then bring up the interface as described below

## Bringing up the interface

Now, bring up the interface

    ifconfig wlan0 up

If your network uses DHCP, I always get an error, but I do get an IP address:

    dhcpcd wlan0

Otherwise, you can enter your IP Manually:

    ifconfig wlan0 STATIC_IP_ADDRESS netmask NETMASK_ADDR

    route add default gw GATEWAY_ADDR

If you do this frequently, I suggest you write a shell script to automate
this behavior.

If you are in an area which you don't know what the SSID of an access point 
is, you can use some of the other `iwconfig` tools. `iwlist` is one of them. 
Type `iwlist --help` to get a full listing of the items which are supported. 
`iwlist scanning` also provides useful information.
