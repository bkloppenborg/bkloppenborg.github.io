---
layout: post
title: "Automatically Backing Up Firefox Bookmarks"
date: 2009-08-26T12:00:55-00:00
draft: false
author: "Brian Kloppenborg"
tags: ["firefox", "backups"]
categories: ["backups"]
---
{% include JB/setup %}

Firefox can automatically export your bookmarks whenever the browser is closed
by setting the following field: Open up the about:config page (i.e. type
`about:config`` in the address bar) and set: 

    browser.bookmarks.autoExportHTML = true

This will export your bookmarks to a file called "bookmarks.html" in your
profile directory (e.g. 
`/home/$USER/.mozilla/firefox/\_uniqueID\_/bookmarks.html` 
for Linux or inside of the 
`C:\\Documents and Settings\\username\\Application Data\`
folder on Windows). That is useful, but if you want the bookmarks in a
convenient locaiton for some other automated backup process (i.e. rsync or
Offline Files) you should also tell Firefox where to place the file using the

    browser.bookmarks.file 
    
setting in about:config. You will need to add the (string) key because it does 
not exist by default:

With the (string) value being the location to which you want the file saved. For
Linux users you simply specify the full path and name of the file (i.e.
/home/$USER/bookmarks.html). For Windows users, you need to do the same but you
must include two slashes between the drive letter and path (i.e. 
`C:\Documents and Settings\username\bookmarks.html``). 

For more information, see Mozilla's documentation:
[browser.bookmarks.autoExportHTML](http://kb.mozillazine.org/Browser.bookmarks.autoExportHTML)
and [browser.bookmarks.file](http://kb.mozillazine.org/Browser.bookmarks.file)
