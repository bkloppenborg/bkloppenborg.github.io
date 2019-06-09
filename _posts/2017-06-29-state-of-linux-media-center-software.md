---
layout: post
title: "2017 State of Linux Media Center Software"
date: 2017-06-29T12:02:26-07:00
draft: false
author: "Brian Kloppenborg"
categories: ["product-reviews", "mediacenter"]
tags: ["Windows Media Center", "DVBLink", "TVHeadEnd", "Video Disk Recorder", "MythTV", "JRiver Media Center", "SageTV", "TVTime", "Plex", "Emby"]
---

Recently a client expressed interest in developing a pre-built Home Theater PC
(HTPC) solution with Digitial Video Recorder (DVR) functionality for Over The
Air (OTA) broadcasts. They considered both low-end devices, like the Raspberry
Pi, and high-end devices like dedicated PCs. As part of their marketing
research, they wanted to have a broader perspective of existing solutions on
Windows, Mac, and Linux platforms. Out of respect for the open-source
community's efforts, our client has kindly agreed to let us publish a subset of
the report that summarizes HTPC solutions with built-in OTA recording and
playback capabilities.

## Overview

As indicated in a 
[2016 Forbes article](http://fortune.com/2016/04/05/household-cable-cord-cutters/), 
up to 20% of US households have dumped cable TV in favor of streaming television
and Over The Air (OTA) services. Without significant capital and access to new,
original, or popular content, it will be difficult to compete with streaming
providers. The presence of low-power set-top-boxes like Tivo, Roku, Apple TV,
NVIDA's shield, and Chromecasts further complicates the market space. As more
households move to cut the cord, one area of opportunity will be the creation of
both low- and high-end, multi-functional Home Theater PC (HTPC) solutions for
consumers that provide both streaming and live television recording and playback
capabilities. To that end, this report examines current market trends, provides
an overview of typical hardware presently available, and presents an analysis of
9 existing HTPC software packages for the Linux operating system that could be
leveraged by small or medium businesses to develop a high-end media center
solution.

## Market Trends and Analysis

One of the most reputable sources of TV viewing information is the Nielson
Agency. They study the trends and habits of media consumption (TV, Radio, etc.)
in more than 100 countries worldwide. In their recent 
[2016 Q1 Nielson Report](http://www.nielsen.com/us/en/insights/reports/2016/the-total-audience-report-q1-2016.html)
they present several key findings about the latest media consumption habits: The
key points of this report is as follows:

* The report partitions audiences into several groups by age (2-11, 12-17,
  18-24, 25-34, 35-49, 50-64, and 65+ years old) and also by ethnicity (White,
  Black, Hispanic, and Asian American).
* TV is the dominant source of entertainment (148 hr / month). App/Web on a
  smartphone (59 hr/month) and AM/FM radio (54 hr/month) are second and third,
  respectively. Time-shifted TV (DVR, 23 hr/month), video on a PC (39.5
  hr/month), and video on a smartphone (2.5 hr/month) barely stack up in
  comparison of well-established content delivery methods.
* Over the last five years, TV consumption has decreased by 1.8 - 39.1% for
  all groups EXCEPT the 65+ group which saw a 4.6% increase.
* Over the last year, TV consumption has decreased by 2.4 - 13.3% for all
  groups younger than 50 years old. These decreases are all fairly large
  relative to reports from one year ago. Ages 50+ have seen a slight increase
  in TV consumption.
* DVRs are present in 50% of surveyed households, but this trend has remained
  constant for the last two years. Meanwhile, streaming video on demand has
  grown by nearly 10%.
* Most households have Cable TV and Internet access in every ethnic
  demographic with fewer than 17% of households relying on broadcast TV alone
  (see Table 9 in the Nielson report).
* Almost all households have a HDTV (94% in the 2016 composite group), at
  least one smartphone (74% in 2016), and about half of all households have a
  tablet (58% in 2016).
* Lastly, 50% of all households subscribe to some form of online streaming
  service. In the 2015 Q1 study, only 42% of households subscribed.

With this information, some general trends can be observed. TV and radio remain
the dominant sources of entertainment. Over The Air (OTA) and Cable TV
consumption is decreasing, streaming services are increasing, and smartphones /
tablets are available in most households. Concerning younger viewers, a 
[report by Marketing Charts](http://www.marketingcharts.com/television/are-young-people-watching-less-tv-24817/) 
investigates generic trends between streaming and live TV for viewers under the
age of 34 years old. Some sources
[claim](http://www2.deloitte.com/us/en/pages/technology-media-and-telecommunications/articles/digital-democracy-survey-generational-media-consumption-trends.html)
that that 19-25 year olds spend 29% of their TV content time streaming and 29%
watching traditional linear TV. Other
[sources](http://ssrs.com/research/ssrs-media-and-technology-survey/) 
suggests that traditional TV still has slight edge over streaming services, but
indicate that streaming services are, in general, increasing in popularity. With
these trends in mind, we argue that future media center solutions should focus
on content delivery to multiple device types. Clearly the television is king in
this respect, but other devices like smartphones and tablets should be
considered. Thus, a set-top-box should provide not only the capability to play
back video to a large TV screen, but also stream the content to one or more
low-powered mobile devices.

## OTA TV tuner support in Linux

Due to (limited) contributions from industry and impressive reverse engineering
efforts, the Linux operating system supports a wide variety of TV capture cards,
tuners, and related devices. Almost all TV capture cards supported in Linux have
drivers found within the Video4Linux (V4L) portion of the Linux Kernel. Almost
all Linux-based DVR software leverages the V4L tree in some form; however, a few
vendor-specific solutions do exist. Supported devices within the V4L tree
include dedicated PCI/PCIe cards, pluggable USB tuners, and network-based
devices. In the context of this review, we used the DViCo Fusion HDTV7 Gold, a
PCIe tuner, the OnAir Solution / Sasem USB HDTV Creator, and a SiliconDust HD
HomeRun Connect which represent devices with strong support within the V4L tree.

### PCIe devices

There are a plethora of PCIe TV tuners on the market. For this work, we used a
DViCo Fusion HDTV7 Dual Express is a PCIe-x1 card we had on hand. The card
includes two XC5000 tuners that are connected to a single F-connector. The card
supports both ATSC and NTSC broadcast TV, analog video via. S-video, and an
external IR receiver for a remote control.

### USB devices

Outside of internal PCIe devices, the second most popular type of tuner are USB
devices. To represent this type of tuner, we used an OnAir Solution USB HDTV
Creator we had on hand. This somewhat obscure device came out in the early
2000's and was among the first USB 2.0 tuners on the market. Depending on when
the tuner was purchased, it could be know it as the Sasem OnAir Creator (the
original company), OnAir Solution USB HDTV Creator (global seller), or
AutumnWave USB HDTV Creator (US subsidiary). This tuner was sold right shortly
before the regulatory mandated DTV switch occurred, thus it supports digital
broadcast TV (ATSC), digital cable TV (QAM-unencrypted), Analog TV (NTSC), and
also features S-Video and RCA inputs. Its claim to fame is that it was the first
external HDTV device featuring USB 2.0 which meant that it could stream full
1080p video (USB 1.x is bandwidth-limited to 720p). Although it is no longer
sold, it remains fully supported in V4L.

### Network devices

The most popular network ATSC tuner is the 
[Silicon Dust HDHomeRun](https://www.silicondust.com/hdhomerun/). 
It comes in three variants (Connect, Extend, and Prime). All units connect to
the network using a Ethernet port. They stream TV content to a computer, DLNA
compatible smart TV, Android media player, Apple TV (4th generation), or game
console. The HDHomeRun devices have been around since at least 2004 and thus are
well respected within the consumer-grade AV market. At present, the fourth
generation Connect variant (HDHR4-US2) retails for around $100 and the Extend
version costs about $130. Meanwhile, the HDHomeRun Prime costs about $180. The
Connect and Extend units are designed for OTA TV and un-encrypted Digital cable
TV. These units feature two tuners that connect to the network using a shared
100 Mbps network interface. The Connect unit streams uncompressed MPEG data to
the client whereas the Extend includes a hardware transcoder (dedicated ASIC)
which converts the raw MPEG stream into H.264 prior to transmission to the
client. Both devices permit streaming over WiFi. The connect requires a network
based on 802.11ac, whereas the Extend's H.264 compression permits it to operate
on an 802.11n network. The HDHomeRun Prime is designed for encrypted cable
streams which require a specialized hardware (in this case a CableCard) to
decrypt the signal. This unit includes a gigabit network interface and includes
three tuners. Streaming content over WiFi requires 802.11ac HD video or 802.11n
for standard definition (SD) respectively. For the purposes of this study, we
purchased a HDHomeRun Connect which was connected to our test machines using
gigabit networking.

## TV Guide Data

A very important thing to note is that US Federal Regulations mandate that all
stations, except local low-power stations, broadcast not only the video and
audio content, but also Program and System Information Protocol (PSIP) metadata
encoded in the raw MPEG stream. Among this data is the Event Information Table
(EIT) which includes titles and program guide data. Although the
[protocol](http://atsc.org/wp-content/uploads/2015/03/Program-and-system-information-protocol-implementation-guidelines-for-broadcaster.pdf)
permits program scheduling up to 16 days in advance to be advertised;
regulations require only the first _FOUR_ EITs be occupied. It is _RECOMMENDED_
that broadcasters provide at least the first 24 EIT entries. In the US, it is
common for US broadcasters to provide 12 - 24 hours of EIT data. In our
experience in the metro Atlanta area, few TV stations broadcast more than 24
hours of scheduling data. Outside of broadcast providers, there are also several
online services which transmit guide data. Because the schedules are
copyrighted, the data are seldom provided for free. A frequently recommended
source of guide data is 
[Schedules Direct](http://schedulesdirect.org/) 
which sells access to the data for $25/year. The proceeds from sales fund open
source projects.

## DVR Software

### Windows Media Center

Although obviously not software which runs on Linux, Windows Media Center (WMC)
has long been a popular PVR solution for cord cutters. Windows Media Center was
supported in Windows 2002 - Windows 8.1. WMC support was dropped from Windows
10; however, it can still be installed using a command-line script. WMC provides
the ability to watch and record live OTA TV from a variety of TV tuners,
including some network devices. WMC use to acquire TV guide data through the
Zap2It service until July 2015 when it switched to Rovi (the folks behind TiVo).
In addition to live TV, WMC lets you watch other videos, listen to music, view
your photos, and even connect to some streaming providers. Although quite
powerful, Windows Media Center does not provide a web interface or any streaming
capabilities out of the box. This isn't much of a problem though because
third-party software, like [Remote Potato](http://www.remotepotato.com/) have
filled in the gaps. Remote Potato doesn't provide a web interface, but its apps
for Windows, iOS, Android, and Windows Phone provide full access to all of the
media on your WMC server.

<div width="100%">
  <img width="24%" alt="Default home screen"
    src="/images/blog/media-center-software-2017/wmc-startup.png"
    class="img-responsive img-thumbnail">
  <img width="24%" alt="Program guide with episode information"
    src="/images/blog/media-center-software-2017/wmc-guide.png"
    class="img-responsive img-thumbnail"> 
  <img width="24%" alt="Episode information screen"
    src="/images/blog/media-center-software-2017/wmc-recording-info.png"
    class="img-responsive img-thumbnail">
</div>

### DVBLogic's DVBLink

[DVBLink](http://dvblogic.com/en/dvblink/) is a commercial (~$60) software
package marketed to be "your very personal TV server". It is an all-in-one
solution that provides support for watching live TV, scheduling recordings,
watching recordings, and transcoding video to watch on other platforms. The
server software runs runs on Windows, Mac, and (Debian-based) Linux operating
systems as well as a select set of Network Attached Storage (NAS) units. The
interface software (to view, schedule recordings, etc.) is a separate
application that runs on Windows, Mac, Linux, Android, and iOS. For the purposes
of this review, we used a demo copy of DVBLink 6.0.0 found on their forums.
Installation of the DVBLink server on Linux is trivial. Simply run the `.deb`
installer and it will copy files and automatically register itself with the
appropriate system initialization script. This was tested both with `init` and
`systemd` configurations. The installer places a "DVBLink" shortcut in the main
application menu. When the program is first executed, it will provide the user
with an opportunity to run a series of installers that expand DVBLink's
functionality. For OTA TV, the "TVSource" plugin is required. DVBLink picked up
both the PCIe and USB tuners, but [several additional steps were
required](http://forum.dvblogic.com/viewtopic.php?f=57&t=28963) to get the
HDHomeRun to function. DVBLink can extract EPG data from transmitted EIT data,
or use one of several external schedule sources. All DVBLink software comes with
trial periods so it can be evaluated risk-free.

<div width="100%">
  <img width="24%" alt="DVBLink Package Manager" 
    src="/images/blog/media-center-software-2017/dvblink-package-center-epg.png"
    class="img-responsive img-thumbnail">
  <img width="24%" alt="DVBLink Package Manager" 
    src="/images/blog/media-center-software-2017/dvblink-package-center-signal.png"
    class="img-responsive img-thumbnail">
  <img width="24%" alt="DVBLink TV Source Dialog" 
    src="/images/blog/media-center-software-2017/dvblink-tv-sources.png"
    class="img-responsive img-thumbnail">
  <img width="24%" alt="DVBLink Channel Editor" 
    src="/images/blog/media-center-software-2017/dvblink-channel-editor.png"
    class="img-responsive img-thumbnail">
  <img width="24%" alt="DVBLink Channel Scanning Dialog" 
    src="/images/blog/media-center-software-2017/dvblink-tv-scan.png"
    class="img-responsive img-thumbnail">
  <img width="24%" alt="DVBLink EPG Info Scanner" 
    src="/images/blog/media-center-software-2017/dvblink-epg-scanner.png"
    class="img-responsive img-thumbnail">
  <img width="24%" alt="DVBLink Setup Guide" 
    src="/images/blog/media-center-software-2017/dvblink-setup-guide-message.png"
    class="img-responsive img-thumbnail">
</div>

Once the necessary software is installed one can access the sources tab where
detected tuners will be displayed. If an EPG subscription service is used, the
data will be automatically populated; however, if OTA EPG data is acquired, the
user will need to 
[follow specific instructions](http://forum.dvblogic.com/viewtopic.php?f=69&t=29128#p99970) 
in order to get it to work. After installation, DVBLink offers a fairly standard
interface for watching and recording TV. This includes the TV guide interface
(h) as well as a method to schedule one or multiple recordings (i). Please note
that the screenshots below captured schedule data from the OTA source, rather
than SchedulesDirect. Thus the content is somewhat more sparse than the other
applications shown.

<div width="100%">
  <img width="24%" alt="DVBLink EPG Overview" 
    src="/images/blog/media-center-software-2017/dvblink-tv-guide.png"
    class="img-responsive img-thumbnail">
  <img width="24%" alt="DVBLink Episode Information" 
    src="/images/blog/media-center-software-2017/dvblink-tv-guide-info.png"
    class="img-responsive img-thumbnail">
  <img width="24%" alt="DVBLink Web Interface" 
    src="/images/blog/media-center-software-2017/dvblink-web-interface.png"
    class="img-responsive img-thumbnail">
  <img width="24%" alt="DVBLink Web Settings Dialog" 
    src="/images/blog/media-center-software-2017/dvblink-web-interface-settings.png"
    class="img-responsive img-thumbnail">
  <img width="24%" alt="DVBLink Web Message Dialog" 
    src="/images/blog/media-center-software-2017/dvblink-web-interface-message.png"
    class="img-responsive img-thumbnail">
</div>

As shown above, DVBLink also includes a web interface that runs on port 8100. It
presents a virtually identical interface to the stand-alone app. It permits you
to view the guide data, schedule recordings, watch recordings, and even stream
live TV. By default the web interface will transcode video to 640x480 pixels at
512 kbps; however, this is configurable via. the "Settings" menu in the web
app. You can even disable transcoding entirely if you so desire. Although the
web interface is quite functional, the software suggests that you will have a
better experience using one of their standalone applications for playing live
and recorded TV.

### TVHeadEnd

[TVHeadEnd](https://tvheadend.org) is a TV recording and streaming server that
functions in Linux, FreeBSD, and Android operating systems. It supports a wide
variety of devices (via. the V4L library) including DTV cards (DVB-S, DVB-S2,
DVB-C, DVB-T, ATSC, ISDB-T, IPTV, and SAT>IP), network tuners, and analog video
capture cards. You can also use IPTV devices. Unlike the aforementioned
projects, TVHeadEnd is open source with development and distribution on the
[TVHeadEnd GitHub Page](https://github.com/tvheadend/tvheadend).  It can be
compiled from source, or installed using the packages available for several
distributions. For the purposes of this review, we used TVHeadEnd 4.1 (from git
commit g55fec0f). TVHeadEnd uses a web-based interface for setup, recording, and
playback. After the software is installed, the web interfaces is at
[http://localhost:9981](http://localhost:9981). Starting with version 4.1,
TVHeadEnd features a guided setup procedure which we found significantly reduced
the complexity of setting up the software. The guide will let you set up an
administrator account, a user account, pick languages, configure your tuners,
scan for channels, and map services. Much other TV tuner packages, TVHeadEnd can
accept guide data from online sources or scrape it from broadcast EIT data.
Screenshots of the installation and configuration procedure are shown below:

<div width="100%">
  <img width="24%" alt="TVHeadEnd Setup Dialog" 
    src="/images/blog/media-center-software-2017/tvheadend-setup.png"
    class="img-responsive img-thumbnail">
  <img width="24%" alt="TVHeadEnd Network Tuner Setup" 
    src="/images/blog/media-center-software-2017/tvheadend-setup-network-tuner.png"
    class="img-responsive img-thumbnail">
  <img width="24%" alt="TVHeadEnd Channel Scanner" 
    src="/images/blog/media-center-software-2017/tvheadend-setup-scan.png"
    class="img-responsive img-thumbnail">
  <img width="24%" alt="TVHeadEnd Service Mapper" 
    src="/images/blog/media-center-software-2017/tvheadend-setup-map-service.png"
    class="img-responsive img-thumbnail">
</div>

Once the software is configured, the user is returned to the EPG tab which is,
perhaps, the worst EPG interface of all of the packages reviewed. All program
data is presented in a massive table without any apparent effort to make the
software more usable. Most of the time TVHeadEnd serves as a backend for other
players, like Kodi, which might explain this non-user-friendly approach.
Recordings can be configured to be standalone or recurring event triggered by a
query (e.g. regular-expression like). After a user schedules a recording, the
dialog simply disappears without any user feedback. In addition to scheduling,
the user interface lets you check on the state of a recording, download the (raw
transport, `.ts`) video file, or delete the recording. Representative
screenshots of these steps are shown below:

<div width="100%">
  <img width="24%" alt="TVHeadEnd EPG Display" 
    src="/images/blog/media-center-software-2017/tvheadend-epg.png"
    class="img-responsive img-thumbnail">
  <img width="24%" alt="TVHeadEnd Recording Status" 
    src="/images/blog/media-center-software-2017/tvheadend-finished-recordings.png"
    class="img-responsive img-thumbnail">
  <img width="24%" alt="TVHeadEnd Recording Profiles" 
    src="/images/blog/media-center-software-2017/tvheadend-recording-profiles.png"
    class="img-responsive img-thumbnail">
  <img width="24%" alt="TVHeadEnd Schedule Recording Dialog" 
    src="/images/blog/media-center-software-2017/tvheadend-schedule-recording.png"
    class="img-responsive img-thumbnail">
</div>

TVHeadEnd also supports live-streaming TV to a web browser. In the screenshots
below we show (i) channel selection dialog and (j) method of choosing the
transcoding technique.

<div width="100%">
  <img width="24%" alt="TVHeadEnd LiveTV Player" 
    src="/images/blog/media-center-software-2017/tvheadend-livetv-channel-selector.png"
    class="img-responsive img-thumbnail">
  <img width="24%" alt="TVHeadEnd Transcoding Options" 
    src="/images/blog/media-center-software-2017/tvheadend-livetv-transcoding.png"
    class="img-responsive img-thumbnail">
</div>

By default TVHeadEnd will record raw (uncompressed) MPEG-2 transport streams
(`.ts`). If the user desires, TVHeadEnd can transcode the video by selecting the
"Configuration -> Recording -> Digital Video Recorder Profiles" tabs and
changing the \`Stream Profile\` to one of the entries beginning with `webtv`.
The `webtv-h264-aac-matroska` profile will encode the video in H.264 format,
while simply packaging up the existing AAC audio stream into a Matroska
container which is probably the best option for most use cases. As previously
mentioned, TVHeadEnd is commonly used by other media center solutions. For
example, [Kodi](https://kodi.tv/) has a plugin that can leverage TVHeadEnd's DVR
capabilities.

### Video Disk Recorder (VDR)

The Video Disk Recorder (VDR) is an open source PVR solution. It was first
released in 2002 and was under active development until 2015. The software is
packaged with most Linux distributions; however, on our Ubuntu test system we
were unable to get it to run. The lack of documentation makes this project
particularly frustrating. The software allegedly supports EPG data from both
subscriptions and OTA sources, support for multiple tuner cards, and timed
recordings. However, we were unable to verify these claims. Playback appears to
be limited to the primary recording device.

### MythTV

[MythTV](https://www.mythtv.org/) has long been a strong contender in the Linux
media center market. It can use all V4L devices and several network tuners, like
the HDHomeRun. One member of our team has used MythTV to record and watch OTA
broadcasts since 2007. Due to its maturity, MythTV is one of the most feature
complete solutions on the market. It supports multiple tuners (with some support
for CableCards), intelligent scheduling with conflict resolution, video
transcoding, commercial flagging, and streaming playback on multiple devices.
For the purposes of this review, we installed MythTV 0.28. Since our initial
review, MythTV 0.28.1 was released. Because most Linux distributions package
MythTV by default, installation is fairly trivial; however, it is not error
free. After installing the software and running the MythTV Backend Setup
(`mythtv-setup`), our tests on Ubuntu 16.04, Ubuntu 17.04, and Mythbuntu found
that the (default) password in `mythtv-setup` and the MySQL database did not
match. This mis-configuration later impacted our ability to run both MythWeb and
WebFrontend. This is fairly easy to fix using the MySQL command line, or a GUI
(like MySQL workbench). Our long-term MythTV user recounts having to manually
address this issue since at least Ubuntu 14.04. After launching
`mythtv-setup` the user will need to go through the capture card, set video
sources, and input connection setup steps at a minimum. During the input
connection stage, the user will also need to run a channel scan for each input
source. Representative images of these steps are shown below:

<div width="100%">
  <img width="24%" alt="MythTV Setup Screen" 
    src="/images/blog/media-center-software-2017/mythtv-setup-screen.png"
    class="img-responsive img-thumbnail">
  <img width="24%" alt="MythTV input dialog" 
    src="/images/blog/media-center-software-2017/mythtv-setup-input.png"
    class="img-responsive img-thumbnail">
  <img width="24%" alt="MythTV capture card setup" 
    src="/images/blog/media-center-software-2017/mythtv-setup-card-selection.png"
    class="img-responsive img-thumbnail">
  <img width="24%" alt="MythTV video-source setup" 
    src="/images/blog/media-center-software-2017/mythtv-setup-video-source.png"
    class="img-responsive img-thumbnail">
  <img width="24%" alt="MythTV channel scanning" 
    src="/images/blog/media-center-software-2017/mythtv-setup-scan.png"
    class="img-responsive img-thumbnail">
  <img width="24%" alt="MythTV channel scan results" 
    src="/images/blog/media-center-software-2017/mythtv-setup-scan-results.png"
    class="img-responsive img-thumbnail">
</div>

While starting the frontend service, we encountered several issues that we
traced to IPV6 name resolution. After disabling IPV6 on the host machine, within
MythTV itself, and manually starting the backend server, things were up and
running. It is not clear if this is an issue with MythTV or our Ubuntu test
system. MythTV has three user interface methods: a Qt GUI (called Myth
Frontend), MythWeb, and WebFrontend. Below we show a series of screenshots from
the QT frontend with the default skin. 
The welcome screen lets you access the most important functionality quickly and
easily. The program guide offers a very familiar interface to any
set-top-box solution from a cable provider. To schedule a recording one
simply needs to select the program and choose one of the presented recording
options. The TV viewing experience is fairly standard compared with most
modern set-top-boxes.

<div width="100%">
  <img width="24%" alt="MythTV main screen" 
    src="/images/blog/media-center-software-2017/mythtv-frontend.png"
    class="img-responsive img-thumbnail">
  <img width="24%" alt="MythTV EPG screen" 
    src="/images/blog/media-center-software-2017/mythtv-frontend-guide.png"
    class="img-responsive img-thumbnail">
  <img width="24%" alt="MythTV detailed episode information" 
    src="/images/blog/media-center-software-2017/mythtv-frontend-record.png"
    class="img-responsive img-thumbnail">
  <img width="24%" alt="MythTV live video playback with overlay" 
    src="/images/blog/media-center-software-2017/mythtv-frontend-tv.png"
    class="img-responsive img-thumbnail">
</div>

In addition to Myth Fronend, MythTV also features two web interfaces. The first
is MythWeb, a PHP-based website that runs on top of standard webservers. As
installed through `apt-get install mythweb`, MythWeb will run on top of Apache;
however, it also ships with configuration files for `lighttpd` and `nginx`.
Because MythWeb is being replaced by WebFrontend, we will not review its
functionality further. WebFrontend is the newest web interfce for MythTV. It is
still in its infancy and not functionally complete compared to MythWeb; however,
we think it is a significant improvement. For example, in the screenshots below
we show the program status window, TV guide, recording dialog, and current
recording windows. Unfortunately, the current version does not support
streaming, but it appears to be planned.

<div width="100%">
  <img width="24%" alt="MythTV WebFrontend home page" 
    src="/images/blog/media-center-software-2017/mythtv-webfrontend.png"
    class="img-responsive img-thumbnail">
  <img width="24%" alt="MythTV WebFrontend EPG dialog" 
    src="/images/blog/media-center-software-2017/mythtv-webfrontend-guide.png"
    class="img-responsive img-thumbnail">
  <img width="24%" alt="MythTV recording dialog" 
    src="/images/blog/media-center-software-2017/mythtv-webfronted-record.png"
    class="img-responsive img-thumbnail">
  <img width="24%" alt="MythTV recorded shows library" 
    src="/images/blog/media-center-software-2017/mythtv-webfrontend-library.png"
    class="img-responsive img-thumbnail">
</div>

In addition to providing a nice interface, WebFrontend also provides an API and
support for third-party plugins. Due to its (accidental?) alignment with
emerging market trends, we anticipate WebFrontend will grow dramatically in
popularity as it matures.

### JRiver Media Center

[JRiver Media Center](https://www.jriver.com) is a closed-source, cross-platform
media center solution that is marketed as "The Most Comprehensive Media
Software". The software runs on Windows, Mac and Linux. A Master license
(cross-platform support) is $70 whereas a Linux-only license is $50.
Historically JRiver was focused on media management, akin to a library, but in
JRiver 20 (or thereabouts) they added support for recording TV. Unfortunately,
this functionality has not been ported to Linux host platforms (even in JRiver
22 which we installed and tested). Linux devices can, however, play content
recorded on a Windows-based machine. Efforts to fund further development of
JRiver's TV functionality through a 
[kickstarter campaign](https://www.kickstarter.com/projects/tvplus/can-jriver-media-center-replace-windows-media-cent)
were unfortunately unsuccessful.

### SageTV

[SageTV](http://www.sagetv.com/) was a closed-source, proprietary DVR and media
center solution for Windows, OSX, and Linux. The initial release of the software
was in 2002 and it garnered significant popularity. In June 2011, SageTV was
acquired by Google and removed from the market. Speculation was that SageTV was
going to be used by Google for their Google Fiber set-top-box; however, they
eventually created their own solution. In 2015, the SageTV source code was
[released under an open-source license and placed on
GitHub](https://github.com/google/sagetv). The project continues to be actively
maintained, with daily commits from a group of ~30 developers. For the purposes
of this review we attempted to use the latest version of SageTV (git commit
2b2b2eb); however, it failed to build using [instructions found on the
wiki](https://github.com/google/sagetv/wiki). Instead we ended up installing the
client and server Debian packages, version 9.1.5.166, found in the [Sage Open
Source Download Service](https://bintray.com/opensagetv/sagetv/SageTV) (see the
"Files" tab). Using the Debian packages, the installation process was flawless.
After launching the client and connecting to the server, SageTV asked several
questions to configure the software. Among those were questions that could have
been detected if the locale of the machine were parsed. Among the questions
includes time zone  information, which was asked three times. We found this need
to be quite odd. SageTV automatically detected our HDHomeRun device, but did not
find either PCIe card or USB device. Until recently, SageTV offered TV guide
data; however, [that service will shut down on July 1,
2017](https://forums.sagetv.com/forums/showthread.php?t=63884). The developers
suggest that users connect to SchedulesDirect to get episode data. While
entering our ScheduleDirect account information, we noticed a second odd aspect
of the software. For a few windows the developers used a  numeric keypad dialog
to facilitate data entry. The keypad does not interfere with letters typed on
the keyboard, but any numbers entered on either the number row or numpad must be
pressed multiple times before the digit will register. This was unexpected and
exceptionally annoying. Much like MythTV, SageTV uses the (legacy) XML interface
to SchedulesDirect and thus requires additional configuration on the
SchedulesDirect website. It also manages its own channel scan rather than
leveraging the built-in channel information on the HDHomeRun. Below we show a
series of representative screenshots for SageTV. These include the client
windows, source wizard, channel scan dialog, channel scan progress
dialog, EPG information dialog, default frontend interface (with a
recommended plugin installation dialog), the TV guide, and recording
dialog. The live TV and playback dialog (not show) are similar to other software
packages listed here, except the user interface appears on the top.

<div width="100%">
  <img width="24%" alt="SageTV server manager" 
    src="/images/blog/media-center-software-2017/sage-client.png"
    class="img-responsive img-thumbnail">
  <img width="24%" alt="SageTV source wizard" 
    src="/images/blog/media-center-software-2017/sage-source-wizard.png"
    class="img-responsive img-thumbnail">
  <img width="24%" alt="SageTV channel setup" 
    src="/images/blog/media-center-software-2017/sage-channel-scanner.png"
    class="img-responsive img-thumbnail">
  <img width="24%" alt="SageTV channel scan" 
    src="/images/blog/media-center-software-2017/sage-channel-scan.png"
    class="img-responsive img-thumbnail">
  <img width="24%" alt="SageTV EPG wizard" 
    src="/images/blog/media-center-software-2017/sage-epg-wizard.png"
    class="img-responsive img-thumbnail">
  <img width="24%" alt="SageTV main screen" 
    src="/images/blog/media-center-software-2017/sage-gui.png"
    class="img-responsive img-thumbnail">
  <img width="24%" alt="SageTV program guide" 
    src="/images/blog/media-center-software-2017/sage-epg.png"
    class="img-responsive img-thumbnail">
  <img width="24%" alt="SageTV detailed program information" 
    src="/images/blog/media-center-software-2017/sage-recorder-ui.png"
    class="img-responsive img-thumbnail">
</div>

### TVTime

[TVTime](http://tvtime.sourceforge.net/) is an open source software package to
view (but not record) live TV streams from a video capture device. It provides a
fairly standard interface consisting of transparent dialog superimposed over the
transmitted video data. The project was last updated in 2005, but still receives
around 33 downloads per week according to statistics on SourceForge. Given that
it has not seen active development for over a decade, we do not consider this
software highly competitive in the current market landscape.

### Plex

[Plex](https://www.plex.tv/) is a well-respected media center solution that was
originally forked from the popular XMBC (now Kodi) software in 2008. Text on
Wikipedia claims the source is now entirely distinct. Plex consists of a
closed-source backend server with various (sometimes open-source) frontend
components. Plex is cross-platform software. The server runs on Windows, Mac,
and Linux and there are frontends for iOS, Android, Windows Phone, various
set-top-boxes (e.g. Roku, Apple TV, NVIDA's shield), and a browser-based web
interface. Plex can be deployed on a computer within a user's home or as part of
the Plex Cloud. Plex offers a free edition which provides media management
capabilities. Additional features, like Mobile/Cloud Sync, Mobile Applications,
Parental Controls, and DVR functionality are enabled through [Plex
Pass](https://www.plex.tv/features/plex-pass/). Subscriptions are available as
$5/month, $40/year, or $150 for a "lifetime" subscription. \[For readers of the
blog: We elected to omit further information on Plex because its capabilities
are very well explained on its website, unlike most of the other software
packages discussed here\] One of the strongest aspects of Plex, in our opinion,
is how well the software is marketed to and focused on the end user. The
installation process was seamless, maintenance and updates well integrated, and
usability second to none. One of our engineers explained it as the Netflix of
HTPC solutions due to its interface and user-focused design. (That same engineer
issues a similarly laudatory remark towards Emby). In the context of current
market trends, Plex is clearly well aligned and a strong competitor to any
future entrants.

### Emby / Media Portal

[Emby](https://emby.media/) (formerly MediaBrowser) is a modern media center
solution that includes media management. Recently, support for recording and
watching live TV was added. Much like Plex, Emby is organized as a backend
server and client frontends. The backend server runs on Windows, Mac, Linux,
FreeBSD, and some NAS solutions whereas the frontend is a browser interface. As
such, it supports live streaming and transcoding of video feeds to web browsers
found on the aforementioned platforms plus Android and iOS. Emby's core product
is free, but some features (the DVR functionality being one of them) are only
available through the [Emby Premiere
program](https://emby.media/premiere.html) . Pricing for Emby Premiere is
$5/month, $50/year, or $100 for a "lifetime" subscription. (Lifetime being [one
major release or some pre-defined period of
time](https://www.reddit.com/r/emby/comments/585w0w/lifetime_lifetime_of_this_version/).)
Emby can be compiled from source, installed from packages, or pulled as a Docker
container. All installation methods are well documented. Initially, we installed
Emby using a Debian package, but this resulted in a web interface that was very
laggy. On a whim, we tried out the Docker image. Not only did this vastly
simplify the installation process, but also significantly improved the
performance of the frontend. For this review we used Emby 3.2.1.0 with a
subscription to Emby Premier to enable DVR functionality. Emby supports M3U
(video streams) and the HDHomeRun out of the box. Support for USB and PCIe
devices can be added through extensions which call out to other software
packages, like TVHeadEnd and MythTV's backend server. Emby receives guide data
exclusively through online sources, like XML TV or Schedules Direct. Much to our
amazement, Emby used HDHomeRun's built-in channel list to populate its channel
data. After reviewing eight other programs, this was a much appreciated feature.
The images below are representative of the Emby user interface. We show a
sample of the setup dialog that has specific modal information for the
HDHomeRun. The user interface, guide, channel listing, recording, series view,
and suggestion pages for live TV are also shown. In addition to supporting
TV-based content, Emby also supports additional media content types like Movies,
Music, Books, Games, Photos, and other mixed content. Due to time limitations,
we did not evaluate these other features.

<div width="100%">
  <img width="24%" alt="Emby server management console" 
    src="/images/blog/media-center-software-2017/emby-setup.png"
    class="img-responsive img-thumbnail">
  <img width="24%" alt="Emby TV tuner dialog for the HDHomeRun" 
    src="/images/blog/media-center-software-2017/emby-hdhomerun.png"
    class="img-responsive img-thumbnail">
  <img width="24%" alt="Emby main screen displaying live TV and movies" 
    src="/images/blog/media-center-software-2017/emby-frontend.png"
    class="img-responsive img-thumbnail">
  <img width="24%" alt="Emby EPG screen" 
    src="/images/blog/media-center-software-2017/emby-guide.png"
    class="img-responsive img-thumbnail">
  <img width="24%" alt="Emby channel dialog" 
    src="/images/blog/media-center-software-2017/emby-channels.png"
    class="img-responsive img-thumbnail">
  <img width="24%" alt="Emby recording dialog" 
    src="/images/blog/media-center-software-2017/emby-recordings.png"
    class="img-responsive img-thumbnail">
  <img width="24%" alt="Emby series view" 
    src="/images/blog/media-center-software-2017/emby-series-view.png"
    class="img-responsive img-thumbnail">
  <img width="24%" alt="Emby suggested episodes based upon viewing history" 
    src="/images/blog/media-center-software-2017/emby-suggestions.png"
    class="img-responsive img-thumbnail">
</div>

Using the Docker installation method mentioned above, our experience with Emby
was exceptional. One of our engineers even exclaimed it was "like Netflix for
TV." Our recording tests resulted in videos that were automatically transcoded
to H.264. This resulted in bandwidth-efficient streams that worked well on our
mobile devices. In prior versions of Emby, one of our team members recalled
experiencing issues streaming live TV that he claimed were due to the demands of
real-time transcoding. We experienced no such issue in v3.2.1.0, even while
streaming two live TV streams and two pre-recorded videos. Because Emby
downloads (and optionally caches) many of its visual elements from the Internet,
we evaluated how the system functions in a disconnected state. As one would
expect, if the cache is disabled the experience is abysmal. When the cache is
enabled, the interface is functional, but slightly degraded. Features such as
actor information, suggestions, rankings, and related material are (obviously)
not available. Most annoyingly, in an Internet disconnected state, the TV guide
data won't load in the web interface, even though logs indicate the data was
previously downloaded by the server. The reason for this last issue is unclear.
In general our experience with Emby was very positive. One of our developers
uses it on a daily basis (hence the well populated list of recordings in the
screenshots above). Given market trends, we consider Emby to be well positioned
to meet the demands of future cord cutters. With that said, there is some
dissent in the community. A few posts on the popular social media site, Reddit,
indicate [some users are circumventing Emby's subscription
model](https://www.reddit.com/r/opensource/comments/3kfcn6/forking_to_enable_subscriberonly_features_in_emby/)
which we argue could harm the Emby team's economic position. There were also
complaints that the Emby developers removed some repositories for plugins from
GitHub. The community argued this was a move to close-source the software;
however, the developers cited a lack of user contributions. For example, the
Android TV App [experienced this
situation](https://emby.media/community/index.php?/topic/42388-android-tv-source-disappeared-again/).
Given the responsiveness of the developers and preponderance of open-source
projects on their GitHub website, we are inclined to believe the developers.

## Summary

There are a wide variety of solutions to create both low- and high-powered
set-top-box devices that run on Linux. Hardware to receive OTA signals consist
of dedicated PCIe cards, USB tuners, and network tuners. Despite widespread
support in V4L, not all HTPC software supports all PCIe and USB tuners.
Similarly, most, but not all HTPC suites support network devices like the
HDHomeRun. In general, our engineers preferred network-based tuners due to their
ability to reduce signal loss due to lengthy runs of coaxial cable, but clearly
this is not suitable for set-top-box solutions. Concerning software, we argue
that Plex and Emby are the solutions that are presently best-aligned with
current and future market trends. MythTV's WebFrontend has potential, but its
development process has, thus far, been exceptionally slow.
