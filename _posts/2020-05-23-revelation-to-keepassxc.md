---
layout: post
title: "Converting from Revelation Password Manager to KeePassXC"
description: ""
category: ["ubuntu", "upgrades"]
tags: ["revelation", "keepassxc", "ubuntu"]
---
{% include JB/setup %}

After upgrading from Ubuntu 18.04 to 20.04 over the weekend, I was surprised
to learn that my go-to password manager since 2008 had been discontinued.
Yes, the package maintainers finally dropped support for 
[Revelation](https://revelation.olasagasti.info/).
However, given that the last update to Revelation was on July 1, 2012, perhaps I
shouldn't have been so surprised after all.

In either case, I was in a pickle. I had over 520 passwords to transition into
something more _modern_ and I certanitly wasn't going to copy/paste each entry.
To make matters worse, Revelation's CSV export functionality ignores the "notes"
field (among others) which I use extenstively in my database.

After weiging my options, I ended up writing a script to convert Revelation XML
Export data, which includes all of the fields, to a KeePassXC-compatable CSV
format. This has a significant downside in that the data are stored in an
unencrypted format during the transition phase; however, that was the only
viable option I could identify.

I posted the script up on GitHub, along with operation instructions
and some photos in hope that it will be useful to others that find themselves
in a similar situation.

https://github.com/bkloppenborg/revelation-to-keepassxc

