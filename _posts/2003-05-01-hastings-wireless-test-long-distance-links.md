---
layout: post
title: "Hastings Wireless: Testing Long-Distance Links"
date: 2003-05-01T00:00:01-00:00
draft: false
author: "Brian Kloppenborg"
categories: ["hastings-wireless", "old-content"]
tags: ["networking", "wifi"]
---

The first objective of Hastings Wireless will be to test the fesability of
long-distance 802.11b networks using consumer hardware. To complete this
objective, we decided to take advantage of a local telephone company who also
offers wireless internet to the surrounding community. When I was first looking
for an ISP I stumbled across 
[Glenwood Telephone Membership Corporation](http://www.gtmc.net)> (GTMC or 
Glenwood Wireless), a local WISP. I was thinking about subscribing to their
service for many reasons --two of which involved my home being outside of the
DSL limit and my home not subscribing to cable TV-- but I decided not use their
service when I discovered that a few of their customers alleged that GTMC
oversold their bandwidth. They do however, have an abundance of 802.11x networks
established COOP grain elevators throughout Adams County. I decided to give long
distance 802.11x networking a shot.

After making a trip down to Blue Hill, NE to talk with GTMC engineers about our
project, in November 2002, we found the location of two GTMC access points
relative to Node 1. I have provided pictures below:

<table border="1" cellpadding="0" cellspacing="0" width="100%">
  <tr>
    <td>
      <img 
        height="301" src="/images/hastings-wireless/pictures/node1_to_GTMC-Hast.jpg" 
        width="654"/>
    </td>
    <td rowspan="2" valign="top">
      <img 
        height="695" src="/images/hastings-wireless/maps/node1_to_GTMC-Hast.gif" 
        width="121"/>
    </td>
  </tr>
  <tr>
    <td valign="top">
       <p>Above Image: Node1 to GTMC-Hastings; 
       Digital Camera. Direction: South<br/>
       Right Image: Node1 to GTMC-Hastings; United States Geological Survey Map, 
       TIFF format.</p>
       <table border="0" cellpadding="0" cellspacing="0" width="100%">
          <tr>
             <td colspan="2"><strong>Node 1 to GTMC-Hastings</strong></td>
          </tr>
          <tr>
             <td>Distance:</td>
             <td>2+ Miles (3.9 Kilometers)</td>
          </tr>
          <tr>
             <td>Type:</td>
             <td>Non-Line of Sight</td>
          </tr>
          <tr>
             <td>Signal Strength:</td>
             <td>Unknown</td>
          </tr>
          <tr>
             <td>Data Rate:</td>
             <td>11 Mbps, max.</td>
          </tr>
       </table>
    </td>
  </tr>
</table>

&nbsp;

<table border="1" width="100%">
  <tr>
    <td>
      <img height="300" 
        src="/images/hastings-wireless/pictures/node1_to_GTMC-Juni.jpg" 
        width="653"/>
    </td>
  </tr>
  <tr>
    <td>
       <p>Above Image: Node1 to GTMC-Juniata; Digital 
       Camera. Direction: West.<br/>
       Below Image: Node1 to GTMC-Juniata; United States Geological Survey Map, 
       TIFF format.</p>
        <table border="0" cellpadding="0" cellspacing="0" width="100%">
          <tr>
            <td colspan="2"><strong>Node 1 to GTMC-Juniata</strong></td>
          </tr>
          <tr>
            <td width="150">Distance:</td>
            <td>4.6 Miles (7.8 Kilometers)</td>
          </tr>
          <tr>
            <td>Type:</td>
            <td>Somewhat Line of Sight</td>
          </tr>
          <tr>
            <td>Signal Strength:</td>
            <td>Unknown</td>
          </tr>
          <tr>
            <td>Data Rate:</td>
            <td>11 Mbps, max.</td>
          </tr>
        </table>
    </td>
  </tr>
  <tr>
      <td colspan="2">
      <img height="138" 
        src="/images/hastings-wireless/maps/node1_to_GTMC-Juni.gif" 
        width="781"/>
      </td>
  </tr>
</table>

Sometime in early May 2003, we established that we could receive signals from both
of the nearby GTMC towers using a DLink DWL-520+ PCI card and a 24 dBi parabolic
grid antenna. Although links were established, external connectivity was limited,
therefore we were unable to test bandwidth. According to the built-in link indicator,
we had connections in the range of 1-11 Mbps.

Therefore, we have established that a long distance link is possible in a NLOS
or a LOS setting. But wait, there is more!

A few weeks later, we decided to test two-way communication between the desktop
computers in which we had installed the DWL-520+ PCI cards. Unfortunately, poor
weather conditions stopped that activity fairly early. Instead, we attempted to
re-establish links to the local GTMC towers (this worked) and searched for
additional access points in the area. One of the additional APs was near the
Imperial Mall, the other of which is the GTMC-Ayr Tower. I am unsure of where or
on what the transmitter is located but its SSID identified it as an GTMC node
located in Ayr, NE. For visualization I have included more pictures:

<table border="1" cellpadding="0" cellspacing="0" width="100%">
  <tr>
    <td heigh="301" width="654">
      <img src="/images/hastings-wireless/pictures/node1_to_GTMC-Ayr.jpg" 
      height="301" width="654"/>
    </td>
    <td rowspan="2" valign="top">
      <img src="/images/hastings-wireless/maps/node1_to_GTMC-Ayr.gif" 
      height="770" width="170"/>
    </td>
  </tr>
  <tr>
    <td valign="top">
      Above Image: Node1 to GTMC-Hastings 
      (Red), GTMC-Ayr (Gold); Digital Camera. Direction: South.<br/>
      Right Image: Node1 to GTMC-Ayr; Global Positioning Map, <a href="http://maps.yahoo.com">http://maps.yahoo.com</a>.
      <table border="0" cellpadding="0" cellspacing="0" width="100%">
        <tr>
          <td colspan="2"><strong>Node 1 to GTMC-Ayr</strong></td>
        </tr>
        <tr>
          <td width="150">Distance:</td>
          <td>12.8 Miles (20.6 Kilometers)</td>
        </tr>
        <tr>
          <td>Type:</td>
          <td>Non-Line of Sight (Over Horizon due 
            to Earth Curvature and elevation**
          </td>
        </tr>
        <tr>
          <td>Signal Strength:</td>
          <td>Unknown</td>
        </tr>
        <tr>
          <td>Data Rate:</td>
          <td>2-11 Mbps</td>
        </tr>
      </table>
    </td>
  </tr>
</table>

**Note:** Although I may boast that this signal was achieved, I have so far been
unable to recreate a link. It could either be due to some strange atmospheric
condition that allowed me to make the link to start with, or the fact that all
of GTMC's networks were undetectable using DLink's auto detect feature. GTMC has
not gone out of business, but they may have detected the pings I sent and since
"stealthed" their networks, who knows?

The data rate is another story. I noticed that the traffic going down Marian
Road interfered significantly with the signal. Therefore I can only estimate the
data rate with a broad range of uncertainty. The lowest data rate that was auto
detected was two megabits, the highest was 11 megabits.

If (and when) I am able to retest this link, I can provide more concrete data. I
hope to narrow down the location of this tower and in turn confirm that it is in
Ayr, NE. The SSID and network hardware type leads me to believe that I have
indeed established a long distance 20 Km link in a NLOS situation.

**Conclusion:*** A 802.11x network link should be able to be established;
however, more data needs to be collected to confirm that the 20 Km link did
indeed exist with feasible data transfer rates.
