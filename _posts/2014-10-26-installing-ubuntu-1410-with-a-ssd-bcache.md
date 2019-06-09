---
layout: post
title: "Installing Ubuntu 14.10 with a SSD bcache"
description: "How to install Ubuntu 14.10 with a bcache SSD"
category: ["linux"]
tags: ["bcache", "ubuntu"]
---
{% include JB/setup %}

The instructions for setting up `bcache` on 14.10 are nearly identical to what
[Wei Dong wrote on his blog](http://www.wdong.org/wordpress/blog/2014/05/28/installing-ubuntu-14-04-to-bcache/), except I had to create a manual addition for the bootloader before
grub2 would recognize the bcache device.

## Installing Ubuntu 14.10

I used the same prescription as Wei Dong, namely we first install the OS to a
small partition (that will later become swap space) and later copy it over to
the bcache device. An important thing to note is that `grub2` does not recognize
bcache disks, thus we must create a separate `/boot` partition on which our
kernels will reside.

Basically we do a standard Ubuntu installation except during the partitioning
stage we create a somewhat weird partition scheme:

    /dev/sda  - 256 GB SSD
    /dev/sda1 - 1 GB ext4 mounted as /boot (probably too big in retrospect)
    /dev/sdb2 - 255 GB ext4 partition, not mounted
    /dev/sdb  - 750 GB 7200 RPM HDD
    /dev/sdb1 - 8 GB, ext4 partition, mounted as root (/)
    /dev/sdb2 - 742 GB ext4 partiton, not mounted
    
The Ubuntu installer will (rightly) complain that we did not create any swap
space, this can be ignored (for now). After this finish doing the installation
and reboot the machine.

## Setting up bcache

Ubuntu 14.10 includes the `bcache-tools` package in the standard repositories.
Thus we no longer need to add a separate PPA to get these programs. After the
software is installed we need to create a `bcache` with the SSD (`/dev/sda2`) 
caching the HDD (`/dev/sdb2`) backing device. Then we will format the bcache
device with the filesystem of our choosing (in this case ext4). All of this can
be done with the following list of commands:

    sudo bash
    apt-get update
    apt-get install bcache-tools
    make-bcache -C /dev/sda2 -B /dev/sdb2
    mkfs.ext4 /dev/bcache0
    
## Migrating the root filesystem

Now we need to copy the existing Ubuntu installation over to the new `bcache`
device. We do this using rsync:

    sudo bash
    mkdir OLD NEW
    mount /dev/sdb1 old    # the old root
    mount /dev/bcache0 new # this would be our new root
    rsync -a old/ new/     # now NEW contains the root
    
    
## Setting up grub2

Now here is where things get tricky. My installation of `grub2` did not detect
any operating systems on the bcache device. Thus using the automatic `grub-install`
method suggested by Wei Dong won't work. Instead we have to create a custom
bootloader entry. Here's an overview of what we need to do:

1. Create a `chroot` environment 
2. Determine the UUIDs for `/dev/sdb1` and `/dev/bcache0`
3. Update `/etc/fstab` with the new UUID for `/dev/bcache0`
4. Reconfigure grub to display the menu on boot
5. Create a custom bootloader entry for `/dev/bcache0` and update grub
6. Test the setup, reboot, convert `/dev/sdb1` to swap
7. Switch back to the automatic grub scripts

### 1. Create a `chroot` environment 

Now here is where things became a little tricky. My installation of grub2 did
not recognize any operating systems on the `/dev/bcache0` device, thus Wei Dong's
solution using `grub-install` would not work. Instead I had to manually create
a bootloader entry in grub2 which, as it turns out, is pretty easy. Lets first
prepare setup a `chroot` system on the new bcache device:

    sudo bash
    mount /dev/sda2 NEW/boot
    mount /dev/sda1 NEW/boot/efi
    mount -o bind /dev NEW/dev
    mount -t proc none NEW/proc
    mount -t sysfs none NEW/sys
    chroot NEW

### 2. Determine the UUIDs for `/dev/sdb1` and `/dev/bcache0`

Next we need to update `fstab` so that the right disks are mounted at startup.
To do this we need to determine the UUIDs of both the old HDD (`/dev/sdb1`) and 
new bcache (`/dev/bcache0`) devices. The UUIDs are the hyphenated alphanumeric
strings in the output from the following commands:

    
    ls -l /dev/disk/by-uuid/ | grep bcache0
    lrwxrwxrwx 1 root root 13 Oct 25 15:57 c596b66a-f27c-4d54-917b-37e578528ed8 -> ../../bcache0
    ls -l /dev/disk/by-uuid/ | grep sdb1
    lrwxrwxrwx 1 root root 10 May 29 21:49 765d6fc0-9ff4-4cf4-95f9-17a6e76ae80c -> ../../sdb1

Here the UUIDs of my devices are as follows:

    /dev/bcache0 c596b66a-f27c-4d54-917b-37e578528ed8
    /dev/sdb1    765d6fc0-9ff4-4cf4-95f9-17a6e76ae80c

### 3. Update `/etc/fstab` with the new UUID for `/dev/bcache0`

This is fairly simple to do. Just edit the `/etc/fstab` file and replace
the UUID for `/dev/sdb1` with the UUID of `/dev/bcache0`. You can open the `fstab`
file using the following:
 
    sudo vi NEW/etc/fstab NEW/boot/grub.cfg 

### 4. Reconfigure grub to display the menu on boot

Edit the `/etc/default/grub` and comment out the `GRUB_HIDDEN_TIMEOUT=0` line.

    sudo vi NEW/etc/default/grub
    
### 5. Create a custom bootloader entry for `/dev/bcache0` and update grub

As mentioned above Wei Dong's solution of updating the UUIDs in 
`NEW/boot/grub.cfg` will not work. This is because 
(1) this file is now automatically generated, thus the next kernel update will
obliterate your changes, and (2) you can't edit the file anyway (even as root!). 
Instead of doing this, we will create a custom bootloader entry to load the
bcache installation of Ubuntu, then remove this entry once the OS is up and
running.

First find the menuentry in `/boot/grub/grub.cfg` that boots your current 
Ubuntu installation (i.e. the one on the hard drive). One of my menu entries 
looked like this:

    menuentry 'Ubuntu' --class ubuntu --class gnu-linux --class gnu --class os $menuentry_id_option 'gnulinux-simple-c596b66a-f27c-4d54-917b-37e578528ed8' {
	    recordfail
	    load_video
	    gfxmode $linux_gfx_mode
	    insmod gzio
	    insmod part_msdos
	    insmod ext2
	    set root='hd0,msdos1'
	    if [ x$feature_platform_search_hint = xy ]; then
	      search --no-floppy --fs-uuid --set=root --hint-bios=hd0,msdos1 --hint-efi=hd0,msdos1 --hint-baremetal=ahci0,msdos1  8cf57a8f-7d20-4d48-b5a3-2ea5700b88c4
	    else
	      search --no-floppy --fs-uuid --set=root 8cf57a8f-7d20-4d48-b5a3-2ea5700b88c4
	    fi
	    linux	/vmlinuz-3.16.0-23-generic root=UUID=765d6fc0-9ff4-4cf4-95f9-17a6e76ae80c ro  quiet splash $vt_handoff
	    initrd	/initrd.img-3.16.0-23-generic
    }
    
Copy this entire block and *append* it to the end of the `/boot/grub.d/40_custom`.
Change the UUID for `/dev/sdb1` to match your bcache device (this change should
only appear in the line beginning with `linux` and switch the title of the
menuentry (in this example `'Ubuntu'` to be something more memorable (like
`'Ubuntu (bcache)'`. Save the file. Now reinstall grub:

    sudo grub-install /dev/sda
    sudo update-grub

Double check that your menuentry was added to the `/boot/grug/grub.cfg` file.

### 6. Test the setup, reboot, convert `/dev/sdb1` to swap

Now we'll test the installation. Reboot your machine and at the grub menu
choose the entry you added and press enter. Check that the bcache device is
being used as the root filesystem using mount:

    $ mount | grep /dev/bcache
    /dev/bcache0 on / type ext4 (rw,errors=remount-ro)

If this works, you can convert `/dev/sdb1` to swap:

    $ sudo bash
    # mkswap /dev/sdb1
    Setting up swapspace version 1, size = 15624188 KiB
    no label, UUID=e35bc636-9944-4dd5-ab3d-6c371b0cb7a8 
    # swapon /dev/sdb1
    # echo "UUID=e35bc636-9944-4dd5-ab3d-6c371b0cb7a8 none swap defaults 0 0" >> /etc/fstab

be sure to update the UUID to match your swap partition!

### 7. Switch back to the automatic grub scripts

Finally we'll undo our customization to grub. First re-enable the 
`GRUB_HIDDEN_TIMEOUT=0` line in `/etc/default/grub`:

    sudo vi NEW/etc/default/grub
    
Remove the custom bootloader lines in `/etc/grub.d/40_custom`:

    vi /boot/grub.d/40_custom

And now redo the bootloader installation:

    sudo grub-install /dev/sda
    sudo update-grub
    
Double check that the menuentries in `/boot/grub/grub.cfg` are correct. If all
is well reboot and you now have a fully functional bcache installation!

### 8. Additional items

As suggested by Rubens Gonçalves below, you should probably check that your
bcache device is working. You can do this by inspecting the results of

    tail /sys/block/bcache0/bcache/stats_total/*

If you get all zeros after a reboot, you may need to attach the bcache 
caching device to the backing device. You can do this by finding the UUID
and echoing it to the bcache attachment:

    ls /sys/fs/bcache # get the UUID
    sudo echo UUID > /sys/block/bcache0/bcache/attach
