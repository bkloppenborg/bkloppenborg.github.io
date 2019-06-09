---
layout: post
title: "Hastings Wireless: Site Survey 1"
date: 2003-11-21-T00:00:01-06:00
draft: false
author: "Brian Kloppenborg"
categories: ["hastings-wireless", "old-content"]
tags: ["wifi-survey"]
---

Using nothing more then a DWL-900AP, a 300 Watt power inverter and a old 120 Mhz
5x86 Laptop we conducted a interference site survey for our long-distance networking
link in Hastings, NE.

## The Path:
  
* Starting in downtown Hastings, NE.
* North on Burlington Ave.
* West on 12th St.
* Imperial Mall Parking Lot, rear side
* South on Marian Road
* East on 7th St

![Survey Path](/images/hastings-wireless/maps/2003.11.21-site-survey.gif)

## Observations

Downtown Hastings is full of 802.11x activity. We parked on 2nd Street and
detected four networks! Three of which did not have WEP enabled, the remaining
one was using WEP. I was pleased to see that at least one group was using WEP.
That was the Masonic Center as identified by their SSID.

Burlington Ave. No network activity was logged.

West on 12th Street At the corner of Burlington and 12th Street, we noticed that
the WLAN light on our DWL-900AP was blinking, we investigated. We found that the
Hastings Public School's Omni Directional Antenna was loud and clear. This link
was previously used by the public schools to connect all of the schools to the
Internet. All of the schools would link up with the middle school and from
there, ESU9. At ESU9, it has been rumored, they have a T3 line. Undoubtedly they
have at least two T1 lines.

Unfortunately due to the high network activity downtown the public schools
stopped using their 802.11x network (to some extent) and installed fiber optic
cables to a few of their buildings. The interference produced by the other
networks caused the Hastings Middle School to ESU9 link to become unstable, and
at times, unusable.

The Imperial Mall Parking Lot (rear side) gave us only one additional link. They
were not using WEP and left their SSID to "linksys" which is fairly depressing
considering the security risk.

We did not find any networks on Marian road, although we have evidence to
believe that there is at least one 802.11x network user located around 7th and
Marian.

No other networks were detected on 7th  street. 

## Conclusion

We did not find any networks that will interfere with our MAN networking
project, unless the downtown networks generate too much signal noise.

  
