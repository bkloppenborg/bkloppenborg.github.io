---
layout: post
title: "Restoring Maxis Ville: my website from 1998"
date: 2019-07-04T09:56:00-07:00
draft: false
author: "Brian Kloppenborg"
categories: ["old-content"]
tags: ["maxis-ville", "backups"]
--- 

When I was in my teens I was obsessed with Maxis video games. I spent a
considerable amount of time to playing SimCity, SimFarm, SimCopter, and Streets
of SimCity. I even constructed a medium-sized website dedicated to these games.
These facts were lost to me until I found several old floppies, hard drives, CDs
and zip disks containing _partial_ copies of my website from 1999, 2004, 2008,
and 2009. Initially I thought my old sites were of no use until I started
searching for related content online. Maxis no longer features most of these
games on their website, most of the large Maxis-focused websites I use to visit
are gone, and many of the games were labeled "abandonware" on the
[Internet Archive](http://archive.org).

I realized that my old, tattered website represented something unique that was
worth preserving. Over the following three weeks I spent my evenings restoring
Maxis Ville to its 1998 glory. I restored the site's hierarchy, replaced broken
links, sorted out content, search for missing files, merged things together, and
discovered how quickly content is lost on the Internet. I now fully encourage
the activities of places like the [Internet Archive](http://archive.org) and
[OoCities](https://www.oocities.org/) which preserve history that would
otherwise be lost.

Below I detail the level of effort and tools that were required to bring
Maxis Ville back from the e-graveyard. If you want to see the final result
of my efforts, go check out [Maxis Ville](https://maxis-ville.kloppenborg.net)
themed in its 1998 liveries.

## Taking inventory

The first step in restoring the website was to take inventory of the
content on hand. 

* 1999 version: This copy was recovered from a zip file found on a 2008-era
  backup CD. It contained several HTML pages and images in a single directory.
  All of the game-specific content was missing and there were no downloads.
  The site used frames, tables with borders, and multi-colored text so it
  was very nostalgic.
* 2004 version: This copy was found on my 
  [home server's]({% post_url 2019-06-06-lack-of-opportunity-pitted-against-tenacious-curosity%})
  hard drive. It had most of the game-specific content organized into
  directories and 13 zip files with DLC. Unfortunately, this version of the
  website required some PHP scripts to add a header, footer, and the navigation
  menu to the rendered files and all of this was missing.
* 2008 version: This copy contained drafts of pages that never made it to the
  main website, several downloads for TheSims and SimCity 3000, and archival
  copies of other websites (GameSpot's SIMply Devine story about Maxis,
  SimStuff, SimEden, and Peter's place in particular). It also held the
  aforementioned zip file of the 1999 website. Unfortunately, the CD was
  burned in ISO 9660 format, so all of the filenames are horribly mangled.
* 2009 version: This version shared many similarities with the 2008 copy;
  however, it contained substantially less content.

<figure>
  <a href="/images/blog/maxis-ville/old-content-inventory.png">
    <img src="/images/blog/maxis-ville/old-content-inventory.png" width="100%"/>
  </a>
  <figcaption>An initial inventory of my files on hand revealed two incomplete
  instances of Maxis Ville from 1999 and 2004, some ancillary files from
  2008/2009, and a ton of missing downloads.
  </figcaption>
</figure>

## Data recovery from the web

Maxis Ville was in pieces. Unlike 
[Hastings Wireless]({% post_url 2019-05-27-hastings-wireless %})
which came back online after a few edits to a `config.php` file, Maxis Ville had
content split among four different root directories, code was missing, filenames
were mangled, and the downloads were (mostly) lost. Fortunately, there were
breadcrumbs to follow.

From the data on hand, I was able to discover the URLs where I previously hosted
my website. Then, using a "History of Maxis Ville" file combined with queries
against [archive.org](http://archive.org) I derived an approximate timeline
for when I switched hosts:

* **1998-01** - Website began as `Streets Online`. Like many sites at the time,
  it was hosted on `GeoCities`. It provided 15 MB of disk space for free and you
  got to be part of a "community" online. I picked the following URL from a
  plethora options: `http://www.geocities.com/TimesSquare/Cavern/1792`.
* **1998-03** - Began writing content for SimCopter and SimCity. Renamed site to
  `Maxis Online`. Website grew too big for one `GeoCities` account, so I set up
  a second account to host my downloads at
  `http://www.geocities.com/TimesSquare/Dome/2346`
* **1999-03** - Started using a lot of external services (CGI scripts) for the
  forms on my website. This peaked my interest in Perl and CGI scripts so I
  moved my site to `WebJump` which offered 25 MB of disk space and a `cgi-bin`
  for free. It appears I also got a subdomain at
  `http://maxis-online.webjump.com`
* **1999-04** - Folks complained about WebJump's advertisements so I sought out
  a new host. I moved my the website to `VirtualAve.net` where I received 20 MB
  of disk space, CGI, Server Side Includes, and a subdomain for free. My site
  was hosted at `http://maxis-online.virtualave.net`
* **1999-05** I quickly filled up my disk quota on `VirtualAve.net` so I picked
  up the `http://members.xoom.com/Maxis_online` for hosting content.
  Unfortunately, Xoom whent offline fairly shortly thereafter.
* **1999-08** - My website had about 75 MB of files stored on four separate
  hosts.
* **1999-08** - Maxis (proper) started to use the phrase "Maxis Online" on their
  website. To avoid a lawsuit, my website was renamed to "Maxis Ville". I opened
  an additional `VirtualAve.net` account at `http://maxis-ville.virtualave.net`.
* **1999-11** - `VirtualAve.net` announces that free hosting will be
  discontinued.
* **1999-11** - Tried out `EziWeb` with my site located at
  `http://free.eziweb.com/maxisville` however this host shut down shortly
  thereafter.
* **2000-01** - Moved MaxisVille to `AFreeHome.com`. They provided 50 MB of disk
  space, but no other features. Website located at
  `http://www.afreehome.com/maxisville`
* **2000-06** - Moved website to `Hypermart`. It provided 20 MB of disk space,
  CGI, Server Side Includes, and a subdomain for free. The website stayed at
  `http://maxis-ville.hypermart.net` until HyperMart announced its closure.
* **2000-06** - Josh van Hulst started SimStuff.com and convinced me to try out
  his server. I moved some content to `http://maxis-ville.simstuff.com`. A
  common online acquaintance of ours, Peter Osorio, convinced me to move my
  Streets of SimCity content to his site at `http://streets.simstuff.com`.
* **2004-01** - Once I got DSL at home, I started to self host the website at
  `http://maxis-ville.homeip.net`. The website remained online here until 2009
  when I took the server down.

With this list in hand, I used the
[Wayback Machine Downloader](https://github.com/hartator/wayback-machine-downloader)
to retrieve as much data from these websites as possible. The VirtualAve and
HyperMart websites returned the most useful information. I also searched
[oocities.org](http://www.oocities.org) for archives of my GeoCities sites,
unfortunately none of that data appears to have been archived. In the end, I got
enough information to reconstruct the appearance of my website in 1998, 1999,
and 2004 as shown in the figure below:

<div width="100%">
<figure>
  <a href="/images/blog/maxis-ville/maxis-ville-1998.png">
    <img src="/images/blog/maxis-ville/maxis-ville-1998.png" width="32%"/>
  </a>
  <a href="/images/blog/maxis-ville/maxis-ville-1999.png">
    <img src="/images/blog/maxis-ville/maxis-ville-1999.png" width="32%"/>
  </a>
  <a href="/images/blog/maxis-ville/maxis-ville-2004.png">
    <img src="/images/blog/maxis-ville/maxis-ville-2004.png" width="32%"/>
  </a>
  <figcaption>Screenshots of MaxisVille's visual evolution from (left to right)
  1998, 1999, and 2004. Despite the use of HTML frames, I restored the website
  to its 1998 appearance for nostalgic reasons.</figcaption>
</figure>
</div>

## Reconstruction

With as much data as possible in hand, I began the arduous, mostly manual,
process of restoring content. The first thing I did was establish the directory
structure for each of the games I would be importing. In almost all cases, the
desired structure for Maxis Ville is similar to the following:

    /
    /streets
    /streets/index.html
    /streets/download/index.html
    /streets/screenshots/index.html

Then I stood up a local webserver in the document root folder using Python's
`SimpleHTTPServer` as follows:

    cd 1999-version
    python -m SimpleHTTPServer

After browsing the website for a few minutes, it was exceptionally clear that
there were a ton of broken links and my files followed no naming convention.
To fix these issues I first renamed all of the files to lowercase:
    
    rename 'y/A-Z/a-z/' *
    
Then I installed and ran [linkchecker](https://wummel.github.io/linkchecker/):

    sudo apt install linkchecker
    linkchecker http://localhost:8000/
    
Upon careful inspection, most of these errors were fairly repetitive and easily
repaired by regular expressions. For example, most of the pages had a link back
to the main Maxis Ville page that was hard coded to
`http://maxis-ville.hypermart.net/main.html` rather than a relative link like
`../main.html`. Here are a few of the tools I used to fix these problems and
how they work:

    # Grep to reursively search for specific strings:
    grep -r "search_string" *
    
    # Grep to search for files NOT containing a specific string:
    grep -riL "search_string" **
    
    # Replace a given string in all of the HTML files in the current directory:
    sed -i "s/old_string/new_string/g" *.html
    
    # Recursively find files, execute sed to do a search-and replacement:
    find . -type f -exec sed -i 's/match_str/replace_str/g' {} +
    
After this, I started importing content, one section at a time, from the 2004,
2008, and 2009 Maxis Ville directories. The HTML pages were quick to import
whereas the screenshot vaults and downloads took some time to restore due to
mangled filenames. Almost all of these problems were trivial (albeit time
consuming) to fix using the command line tools mentioned above and two
of my favorite text editors, `vim` and [spacemacs](http://spacemacs.org).

## Cleaning up the HTML

While fixing various pages I noticed several problems with my HTML files.
Foremost, instead of using HTML heading (e.g. `<h1>`) tags, I used repeated
`<big>` tags for section headings. To fix this issue I used a regular expression
to find and replace instances of 3 or more `<big>` tags:
  
    # Find impacted files
    grep -E -r "(<BIG>){3,}" *
    
    # Replace the <BIG> and </BIG> tags with <h1> and </h1>:
    find . -type f -exec sed -i -E 's/(<BIG>){3,}/<h1>/g' {} +
    find . -type f -exec sed -i -E 's/(</BIG>){3,}/</h1>/g' {} +
    
I also had a variety of `<style>` elements on my pages which made the website
appear inconsistent. To fix this issue, I used Perl's expanded regular
expression capabilities to perform string substitutions across multiple
lines. In the command below, I replace the entire `<STYLE>` section
with the string "STYLE_REPLACE":

    # Replace all HTML <STYLE> information with the string "STYLE_REPLACE"
    perl -i -p0e 's/<STYLE>.*<\/STYLE>/STYLE_REPLACE/se' *.html
    
Then I used `find` and `sed` to replace `STYLE_REPLACE` with a link to a 
CSS sheet:

    find . -type f -exec sed -i 's/STYLE_REPLACE/<link rel="stylesheet" type="text\/css" href="\/style.css">/g' {} +

Note that the forward slashes (`/`) are escaped using a backslash (`\`) in the
above command.

Lastly, I added copyright notices to the bottom of all of my pages. To do
this, I replaced the `</body>` tag with the copyright notice plus `</body>`
tag as indicated below:

    find . -type f -exec sed -i 's/<\/body>/<p align="center">Copyright \&copy; 1999, 2019 Brian Kloppenborg. All Right Reserved.<\/p><\/body>/g' {} +
    
With these changes made and a majority of the content restored, I placed
[Maxis Ville](http://maxis-ville.kloppenborg.net) on a webserver and
gave it a subdomain.

## Conclusion and next steps

By carefully inventoring the data on hand, scraping the web for old content and
the use of various command line tools, I was able to restore 
[Maxis Ville](http://maxis-ville.kloppenborg.net) to its 1998 glory with one
major exception: DLC. Streets of SimCity was hit particularly hard, as can be
seen in the table below:

{:.table}
| Game               | Type           | Quantity  |
| =====              | =====          | ========= |
| SimCity 2000       | Cities         | 2/27      |
| SimCity 3000       | Cities         | 1/1       |
| Streets of SimCity | Tracks         | 0/6       |
| Streets of SimCity | Skins          | 6/95      |
| Streets of SimCity | Cities         | 5/5       |
| Streets of SimCity | Scenarios      | 6/6       |
| SimTower           | Towers         | 0/6       |
| All                | Game Demos     | 4/5       |

Over the next few weeks I'm going to attempt to locate the missing content on
other sim game websites and/or track down the original authors. Wish me luck!
