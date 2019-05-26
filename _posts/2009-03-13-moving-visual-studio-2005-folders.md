---
layout: post
title: "Moving Visual Studio 2005 Default Directories"
date: 2009-03-13T13:03:41-00:00
draft: false
author: "Brian Kloppenborg"
tags: ['visual studio', 'windows']
categories: ['programming']
---
{% include JB/setup %}

In 2008 I transitioned to Linux for almost all of my day-to-day activities.
I have set up a fairly nice backup solution which uses `rsync` to backup
my drive at regular intervals (using `cron`). Unfortunately, a few of my
software development activities still use Visual Studio 2005 which requires
a completely separate backup solution.

In order to unify my backup strategy, I want to move the default Visual
Studio directory from the `C:` drive to my `H:` drive which is a VirtualBox
share from my Linux home directory. Fortunately, the solution was fairly simple
and only required a few registry modifications.

To move the default directory for Visual Studio, open up regedit and change the
keys found in

```
HKEY_CURRENT_USER\Software\Microsoft\MSDN\8.0 HKEY_CURRENT_USER\Software\Microsoft\VisualStudio\8.0
```

to point to the directory of your choosing. For my use case, I changed a
majority of these directories to `H:\Programming\Visual Studio 2005\Projects`
except for the `MyDocumentsLocation` key which I set to `%USERPROFILE%\My Documents`

Now, you might be wondering if I notice a performance hit by compiling over a
network drive. Because the VM is using a host-shared folder, there probably is a
short increase in build time, but it is not noticeable on my machine (which
supports hardware-based virtualization).
