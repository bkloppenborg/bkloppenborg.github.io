---
layout: post
title: "Six years evaluating backup software"
date: 2019-06-08T14:05:00-07:00
draft: false
author: "Brian Kloppenborg"
categories: ["backups"]
tags: ["deja-dup", "crashplan", "bacula", "urbackup", "duplicity", "duplicati", "borg-backup", "restic"]
---

Over the last six years I've tried out a variety of synchronization, mirroring,
and backup software looking for one that does not preclude application of the
3-2-1 backup strategy while supporting multiple operating systems and I'm happy
to report I've found an ideal solution.

## My data loss story

In 2013, I had four computers that continually synced of data using
[SyncThing][4]. The data was approximately 400 GB in size
and was comprised of astronomy data, photos, music, and video. Two of the four
computers used `btrfs` RAID 1 mirroring whereas the other two machines had
single `ext4` disks. The `btrfs` disks were scrubbed weekly. The most critical
data was backed up on DVD whereas the vast majority relied on SyncThing's
file versioning capability which I configured to keep 10 copies of modified
files. Unfortunately, the hard disk controller the drive in a single disk
machine started to fail. This produced which produced erroneous metadata and
checksums on some of the files. As one would expect, SyncThing propagated these
corrupt files to the other machines. 

By the time I caught the issue, approximately 28% of my images and 10% of my
music files were corrupt. Fortunately my DVD backups had most of the images
and all of the music, so no data was lost.

Lesson: Synchronization services are not a backup. Use them to move files. Do
not rely on them for backups or long-term storage.

## Needs and desires

After much experimentation with many backup products, I eventually derived the
following list of desires and requirements for my backup software.

{:.table}
| Category                  | Desire                  | Requirement |
| ------------------------- | ----------------------- | ----------- |
| Hosting method            | Self hosted             | Yes         |
| Operating systems         | Windows, Linux, Android | Yes         |
| Destinations              | local, remote           | Yes         |
| Multiple destinations     | Yes                     | Yes         |
| Encryption                | Yes                     | No          |
| Deduplication             | Yes                     | Yes         |
| Compression               | Yes                     | No          |
| Interface                 | cli                     | No          |
| Scheduling Method         | Any                     | Yes         |

## Backup strategy and software

After the aforementioned debacle, I decided to implement the 
[3-2-1](https://www.backblaze.com/blog/the-3-2-1-backup-strategy/)
backup strategy. This technique proposes that one should keep 3 copies of
important files on 2 different storage media with 1 copy being off-site.
Although some advocate a
[3-2-2](https://www.unitrends.com/blog/3-2-1-backup-sucks)
strategy instead, the United States Computer Emergency Readiness Team
[regards](https://www.us-cert.gov/sites/default/files/publications/data_backup_options.pdf)
3-2-1 as sufficient for most most cases.

To implement this solution, I evaluated 13 potential software solutions: five
options were synchronization software paired with disk snapshots and eight were
dedicated backup software. Below I've provided some brief commentary about each
product and my conclusions about its use.

### Synchronization software

As stated above, synchronization software is NOT a backup solution. However, if
the sync software is used to get data onto a machine with appropriate backup,
this may be sufficient to meet your needs. These products have been extensively
reviewed elsewhere, so I won't comment on them further here.

{:.table}
| Software          | Interface | OS                  |
| ----------------- | --------- | ------------------- |
| [rsync][1]        | cli       | Windows, Mac, Linux |
| [DropBox][2]      | gui, web  | Windows, Mac, Linux |
| [Google Drive][3] | gui, web  | Windows, Mac        |
| [SyncThing][4]    | web       | Windows, Mac, Linux |
| [Resilio][5]      | gui       | Windows, Mac, Linux |
|                   |           |                     |

<!-- MarkDown reference-style links to shrink table width -->
 [1]: https://rsync.samba.org/
 [2]: https://dropbox.com
 [3]: https://www.google.com/drive/
 [4]: https://syncthing.net/
 [5]: https://www.resilio.com/

### Backup software

In total, I evaluated about 20 backup products over the last six years. For the
vast majority of products, the evaluation period was rather short as I discovered
desirable features that the products were missing. For those products that offered
a sufficient number of features, I ran most for at least a month to investigate
their (quasi) long-term use.

In the table below, I summarize the salient features of each product. I intend on
re-reviewing each of these products in the near future to provide more detailed
information, backup/restore benchmarks, anti-tampering information, and general
opinions on the software's operation.

{:.table}
| Software          | Interface | Client OS           | Destinations                 | Encryption | Compression | Deduplication | Multi-Site | Mountable |
| ----------------- | --------- | ------------------- | --------------------         | ---------- | ----------- | ------------- | ---------- | --------- |
| [Deja Dup][6]     | gui       | Mac, Linux          | local, remote, cloud         | Y          | Y           | N             | N          | N         |
| [CrashPlan][7]    | cli, gui  | Windows, Mac, Linux | local, remote, cloud         | Y          | Y           | Y             | Y          | N         |
| [Bacula][8]       | cli, gui  | Windows, Mac, Linux | server                       | Y*         | Y           | Y*            | N          | N         |
| [UrBackup][9]     | gui, web  | Windows, Mac, Linux | server                       | N          | Y*          | Y             | N          | N         |
| [Duplicity][10]   | cli       | Mac, Linux          | local, remote, cloud         | Y          | Y           | N             | N*         | N         |
| [Duplicati][11]   | gui, web  | Windows, Mac, Linux | local, remote, cloud         | Y          | Y           | N             | N          | N         |
| [Borg Backup][12] | cli       | Mac, Linux          | local, remote                | Y          | Y           | Y             | Y*         | Y         |
| [restic][13]      | cli       | Windows, Mac, Linux | local, remote, cloud, server | Y          | Y           | Y             | Y*         | Y         |

<!-- MarkDown reference-style links to shrink table width -->
 [6]: https://wiki.gnome.org/Apps/DejaDup
 [7]: https://www.crashplan.com/en-us
 [8]: http://sourceforge.net/projects/bacula/
 [9]: https://www.urbackup.org/
 [10]: https://wiki.debian.org/Duplicity
 [11]: https://www.duplicati.com
 [12]: https://borgbackup.readthedocs.io/
 [13]: https://restic.net/

## The final (?) solution

The backup solutions which best fit my use cases were [Borg Backup][12],
[CrashPlan][7], and [restic][13]. I evaluated these products for a period of
period of at least two years and subjected them to ever-increasing quantities
of data. After CrashPlan 
[ended its support for home users](https://www.crashplan.com/en-us/consumer/nextsteps/)
in 2018 Borg and restic became my primary backup options. 

Due to its cross-platform backup support, restic is now my backup tool of
choice. In forthcoming blog posts, I'll describe how to set up a dedicated
restic server and the restic clients.
