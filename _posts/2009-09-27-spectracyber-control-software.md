---
title: "SpectraCyber Control Software "
date: 2009-09-27T16:08:50-00:00
draft: false
author: "Brian Kloppenborg"
tags : ['spectracyber"]
categories : ["astronomy"]
---
{% include JB/setup %}

Much to my amazement, in early 2009 I was contacted about the code and my
undergraduate senior project entitled "Design, Construction, and Implementation
of a Radio Telescope to study Neutral Hydrogen." Because of this inquiry, I have
posted a summary of my presentation to the Nebraska Academy of Sciences on
my DU portfolio page.

The control software is written for the 1st generation SpectraCyber units in C#
and is packaged as a Visual Studio 2005 project. The software is presently
configured to push data into a database, but can be changed to use a flat file
with a little effort. Additionally, the software can be made to run under MONO
(for Linux and Mac compatibility) by changing only a few lines in the RS-232
communication code. Lastly, the code was written to be general enough to allow
for use with the SpectraCyber II units, although some additional work (mostly
refactoring) will be required.

You can obtain a copy from the 
[project's GitHub page](https://github.com/bkloppenborg/spectra-cyber)
Please feel free to send me an email with any questions.
