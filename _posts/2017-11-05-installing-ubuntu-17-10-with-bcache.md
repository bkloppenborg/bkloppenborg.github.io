---
layout: post
title: "Installing Ubuntu 17.10 with bcache support."
description: ""
category:
tags: [bcache]
---
{% include JB/setup %}

This post is a continuation of my `bcache` installation instruction series that
began three years ago with 
[Ubuntu 14.04]({% post_url 2014-10-26-installing-ubuntu-1410-with-a-ssd-bcache %})
This edition includes instructions for setting up `bcache` on Ubuntu 17.10.
Unfortunately, Ubuntu still has not included `bcache` support in the initial
`initramfs`, so the instructions remain largely unchanged, save a slight change
to deal with the new systemd-based DNS system.

## Overview

Before we get going, not that this is only valid for a *new* installation of
Linux as we delete all file system information. If this isn't what you want to
do, I suggest you check out
[flashcache](https://github.com/facebook/flashcache)
or EnhanceIO which will let you migrate a live system.

Here are the major steps:

1. Boot the Ubuntu installer
2. Create a partitions for `/boot`, the backing, and cache devices.
3. Create the `bcache` device
4. Install Ubuntu onto `/dev/bcache0`
5. While still in the live CD, `chroot` into the new installation
6. Install `bcache-tools` and re-generate `initramfs`
7. Reboot into a fully functional system.

Properly acknowledging my sources, there are two critical posts on StackOverflow
that made me think I could get away with this scheme:
[alex's answer on how to setup bcache](http://askubuntu.com/questions/523817/how-to-setup-bcache) and
[Lekensteyn's answer on how to restore kernels](http://askubuntu.com/questions/28099/how-to-restore-a-system-after-accidentally-removing-all-kernels)
Lastly, be aware that Grub (and Grub2) do not support bcache, so you will need a
separate `/boot` partition.

## Partitioning

First, if you have used this system for anything important, back up
your data. We'll be erasing everything shortly.

Now, boot into the Ubuntu installer and remove any unnecessary partitions. You
can use `fdisk` on the command line or the `gparted` GUI for this. Now, lets
assume that your SSD is `/dev/sda` and your hard disk is `/dev/sdb`. Create the
following partitioning scheme:

    /dev/sda1 - 1024 MB, EXT4, used for /boot
    /dev/sda2 - any format, for cache
    /dev/sdb1 - EFI partition (if your machine needs it)
    /dev/sdb2 - swap
    /dev/sdb3 - any format, backing partition

Don't worry about doing a deep format of the caching and backing partitions as
we'll wipe these shortly. If you made any major changes to the partition tables,
you might need to reboot before you can proceed. `gparted`, in particular, will
let you know if this is the case.

## Loading bcache, creating device

First, connect to the Internet. Make sure the connection is working. Next open
up a terminal and wipe the cache and backing partition file systems:

    sudo wipefs -a /dev/sda2
    sudo wipefs -a /dev/sdb3

Next we will install `bcache-tools` and create the `bcache` device.

    sudo apt-get update
    sudo apt-get install bcache-tools
    sudo make-bcache -B /dev/sdb3 -C /dev/sda2
    sudo mkfs.ext4 /dev/bcache0

Notice the command to `make-bcache` used the HDD partition, `/dev/sdb3`,
as the backing (`-B`) device and the SDD partition, `/dev/sda2`, as
the cache (`-C`) device.

## Installing Ubuntu

WITHOUT rebooting, run the Ubuntu installer from the desktop. When you get to
the installation type screen which lets you pick how to install the OS (e.g. the
page that says "Erase disk and install Ubuntu" or "Something else") choose to do
custom partitioning.

In the partitioning dialog configure the following:

    /dev/bcache0 - format EXT4, use as /
    /dev/sda1    - format EXT4, use as /boot
    /dev/sdb1    - EFI partition (if your machine needs it)
    /dev/sdb2    - swap

Proceed with the installation as normal. When it completes *DO NOT REBOOT* as
the `initramfs` installed by the live CD does not have the `bcache` kernel
module. If you accidentally rebooted, simply go back in to the live image,
install the `bcache-tools` package as described above and continue with the
instructions below.

## Installing bcache on the new installation

Here is where things get tricky. What we're going to do is switch to the new
operating system without booting and install some software to get `bcache-tools`
installed and a new `initramfs` generated so the computer will boot.

First we are going to create a valid `chroot` environment. We start by mounting
several directories from the new installation into specific subdirectories in
order to create the directory structure Ubuntu Linux expects:

    sudo mount /dev/bcache0 /mnt
    sudo mount /dev/sda1 /mnt/boot
    sudo mount --bind /dev /mnt/dev
    sudo mount --bind /proc /mnt/proc
    sudo mount --bind /sys /mnt/sys

Because we will need Internet access, we need to copy the DNS configuration from
the live CD into the `chroot` environment:

    sudo cp /etc/resolv.conf /mnt/etc/resolv.conf

Next we put ourselves into the `chroot`:

    sudo chroot /mnt

Now we are effectively within the new installation's file system. So all we need
to do is install `bcache-tools`

    sudo apt-get update
    sudo apt-get install bcache-tools

After the package is installed, you should notice that the `initramfs` is
re-generated and installed. You can check the timestamps on the files in `/boot`
against `date` to confirm this is the case.

Now we clean up. Exit the `chroot`, cleanly dismount the file system,
and reboot:

    exit
    sudo umount /mnt/sys
    sudo umount /mnt/proc
    sudo umount /mnt/dev
    sudo umount /mnt/boot
    sudo umount /mnt
    sudo reboot

## Post-install

Restore the default `resolv.conf` configuration

    sudo ln -s /run/systemd/resolve/stub-resolv.conf /etc/resolv.conf

With any luck, your machine will reboot normally and you will have a fully
functional Ubuntu installation with `bcache` out of the box without all the work
of previous methods.
