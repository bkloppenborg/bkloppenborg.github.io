---
layout: post
title: "A lack of opportunity pitted against tenacious curosity"
date: 2019-06-06T07:04:39-07:00
draft: false
author: "Brian Kloppenborg"
categories: ["life", "hardware"]
tags: ["web-development"]
---

## Introduction

While doing some spring cleaning, I found a hard drive which contained backups
of all of my websites from the early 2000s and a few photos of the equipment
which I used to host said sites. Seeing these things inspired me to resurrect
some of this content (see [tag: old-content](/categories.html#old-content-ref)).
In doing so, I realized that learning how to code and host my own services
at such a young age forged a career path that was unlikely for youth in rural
Nebraska.

## A lack of opportunity

In the late 1990s and early 2000s, rural Nebraska provided few opportunities for
tech savvy youth to garner high-tech skills. My high school had a few vocational
courses which catered to local industry, but nothing resembling software
development. There simply wasn't a need: the largest employers in the region
were in the fields of health care, education, and manufacturing. There was also
a considerable number of agricultural companies and the usual smattering of
retail shops. Although all of these companies used software, few were looking
for folks that could code. In fact, when I was looking for my first full-time
job the only advertised software development gig was 60 miles away! This
situation has changed somewhat in the last 20 years: my high school offers
a "Web Page Design & Development" course (which teaches HTML, CSS, and
JavaScript) and there are a few companies which have entry-level software
development positions advertised online.

Unfortunately, the lack of high-tech jobs in my home town in the 2000s meant
that most of the youth settled for low-tech jobs.A typical career would start in
agriculture, working for farmers, around the age of 14. Typically, this would be
[detasseling corn](https://en.wikipedia.org/wiki/Detasseling). Once you reached
16, you would get a job in retail, waiting tables at restaurants, or working in
the manufacturing sector. For example, I went from detasseling to working in a
lumber store stocking shelves. My wife worked in fast food. One of my peers made
a ton of money welding for a local manufacturer.

## Tenacious curiosity

Although my career started along a similar trajectory to my peers, there is one
important distinction: I'm exceptionally curious about technology. In high
school I learned how to program the TI-83 and build web pages. My first website
was MaxisVille, a website dedicated to 
[SimCity 2000](https://en.wikipedia.org/wiki/SimCity_2000), 
[SimCopter](https://en.wikipedia.org/wiki/SimCopter), and
[Streets of SimCity](https://en.wikipedia.org/wiki/Streets_of_SimCity).
The site started out on 
[GeoCities](https://en.wikipedia.org/wiki/Yahoo!_GeoCities) in 1998, moved to 
VirtualAve.net in late 1999 as they provided 
[Server Side Includes](https://en.wikipedia.org/wiki/Server_Side_Includes) 
for free. In 2000 I moved the website to HyperMart.net and then SimStuff.com
as free hosting providers were becoming less capable. In 2004, I moved all
of my websites to a server (pictured and described below) I hosted at home on a
128 kbps down / 64 kbps up connection. Doing so let me develop the following
skills:

* Perl scripting to search Apache logs for missing pages and generate
  rudimentary page view information.
* PHP and MySQL to make dynamic web pages.
* Installation, configuration, and maintenance of software on Linux.
* The ability to inspect my code for security vulnerabilities (suffice it to
  say, I found some).

Later I would go on to use this server to make money. I built an entire content
management system for [Hastings Head Start](http://hshn.org/) in 2006, created a
web-enabled vehicle maintenance tracking system in 2009, constructed a web
application to visualize and sell window cornices for Designers Index, and
installed a modern Content Management System for the 
[Nebraska Head Start Association](https://neheadstart.org/)
in 2010. After moving away in 2012, administration of most of these websites
were taken over by local businesses. 

## Conclusion

Since then, I've continued to use my technological skills to do awesome things.
Most recently, my image reconstruction software from 2010 was cited in the
[M87 black hole image](https://iopscience.iop.org/article/10.3847/2041-8213/ab0e85/meta)
paper in Nature. Now, I wouldn't claim anything I've done to be as amazing as
the 16-year old who earned
[$200k by developing a Twitter App](https://medium.com/tech-product-and-life/how-i-made-200-000-when-i-was-16-years-old-304f0e87cfb6)
or most of the
[other success stories](https://news.ycombinator.com/item?id=20093676)
by young individuals posted on on HackerNews; however it wasn't bad start for a
kid from rural Nebraska.

<hr>

## Epilogue: In memory of my first webserver

<figure>
  <a href="/images/blog/acer-aspire-2190m.jpg">
    <img src="/images/blog/acer-aspire-2190m.jpg" width="50%"
     alt="The Acer Aspire 2190m used to host my websites from 2004-2009"
     style="float: right;"/>
  </a>
  <figcaption>
    My first home server: an Acer Aspire 2190m hosted on a 128/64 kbps Internet
    connection. This modest machine (see specs below) hosted all of my websites
    from 2004 - 2009.
  </figcaption>
</figure>

My first webserver was a used, six-year-old
[Acer Aspire 2190m](https://web.archive.org/web/20010109102200/http://www.acersupport.com/desktop/aspire/html/as21xx_specs.html)
desktop I received from a neighbor in exchange for removing malware from their
new computer. As you can see from the specifications below, the machine was
quite modest: it ran RedHat Linux 9.0 on a 333 MHz processor with 196 MB of RAM
and a 4.3 GB hard drive. My Internet connection was similarly modest: 128 kbps
down / 64 kbps up. Despite its low power, it was still a great machine for
learning and experimenting.

{:.table}
|:----------------- |-------|
| Processor        | AMD K6®-2 333MHZ Processor                         |
| Cache            | 512K  |
| Memory           | 196 MB Expandable to 256 MB |
| Video            | ATI 3D Rage IIC graphics accelerator 4MB video memory |
| Storage          | 4.3GB IDE Hard Drive <br> One 3.5" 1.44MB diskette <br> 32x maximum CD-ROM drive <br> Aspire Desktop HSF-SP          |
| Modem            | 56Kbps modem (V.90 ITU and K56Flex standards) |
| Ethernet         | 10/100 PCI Adapter (added) |
| Ports            | 4 USB ports (2 open), Parallel printer port |
| Operating System | RedHat Linux 9.0 with Linux Kernel 2.4.20 |
| Software         | httpd (Apache) 2.0.40 <br> PHP 4.2.2 <br> MySQL 3.23.54         |
