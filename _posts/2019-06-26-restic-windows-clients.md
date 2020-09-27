---
layout: post
title: "Setup, configuration, and automation of restic for Windows clients"
date: 2019-06-23T08:22:00-07:00
draft: false
author: "Brian Kloppenborg"
categories: ["backups"]
tags: ["backups", "restic"]
--- 

This is the fourth post in a four-part series which discusses my backup solution
consisting of a dedicated [Restic](https://restic.net/) backup server running
the [Restic REST server](https://github.com/restic/rest-server). This post
discusses how to install and automate restic backups on Windows.

The previous posts in this series include:

* [Evaluating backup solutions]({% post_url 2019-06-08-six-years-evaluating-backup-software %})
  in which I discuss a variety of cross-platform backup solutions and why
  I ultimately settled on Restic.
* [Configuration of the backup server]({% post_url 2019-06-12-restic-backup-server %})
  where I discuss setting up dedicated backup machine with a `btrfs` RAID1 filesystem,
  automation of `btrfs` scrubbing using `systemd` timers, and the installation of
  Restic and the Restic REST server.
* [Linux client configuration]({% post_url 2019-06-15-restic-backup-linux-clients %})
  wherein the installation, configuration, and automation of restic is discussed.
  Automation is performed using `systemd` timers.
 
## Installation

Download appropriate release from
https://github.com/restic/restic/releases

Per the [installation instructions](https://restic.readthedocs.io/en/latest/020_installation.html#id2)
extract the zip file to the following directory:

    %SystemRoot%\System32 
    
to avoid setting enviornmental variables. This is normally the
`C:\Windows\System32` directory

As admin

    cd C:
    cd C:\Windows\system32
    mklink restic.exe restic_0.9.5_windows_amd64.exe 
    
## 
