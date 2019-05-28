---
layout: post
title: "Hastings Wireless: Building a 24 dBi omni-directional antenna"
date: 2004-03-15T00:00:01-06:00
draft: false
author: "Brian Kloppenborg"
tags: ["antennas", "omni-antenna"]
categories: ["hastings-wireless", "old-content"]
---

Over the weekend we constructed a 24 dBi omni-directional antenna for 2.4 GHz
wireless signals. The antenna was constructed using

* N-type female panel connector
* K&S Brass tubing
* 1" PVC tubing and ends 

We followed instructions for the 
[Aerialix OM2400 antenna](/downloads/aerialix-om2400-all-inst.pdf) 
and used their
[alignment template](/downloads/aerialix-alignment-template-sheet.pdf)
to ensure our implementation matched their design. We also found their
[coil construction suggestions](/downloads/aerialix-antenna-hints.zip)
to be quite useful. The end result turned out great:

![Custom-built 24 dBi omni-directional Antenna](/images/hastings-wireless/antennas/24dbi-omnidirectional-antenna.jpg)

To protect the antenna outside, we encased it inside of 1" diameter PVC.
However, the physical stress placed upon the decoupler frequently exceeded
the strength provided by the solder, so we had to modify our design.
Originally, the decoupler was soldered directly to the N-connector as 
follows:

![Decoupler](/images/hastings-wireless/antennas/24dbi-omni-decoupler.jpg)

However, after mounting inside of the PVC endcap, we added a ground wire
between bolts threaded through the N-connector's mounting points and
the decoupler:

![Endcap](/images/hastings-wireless/antennas/24dbi-omni-inside-endcap.jpg)

In addition to the 24 dBi omni, we also soldered a small 1/4 wave antenna
together for later field testing:

![Quarter Wave N-connector](/images/hastings-wireless/antennas/quarter-wave-n-connector.jpg)

## Related Material

[A 2.4Ghz Vertical Collinear Antenna for 802.11 Applications](https://www.nodomainname.co.uk/Omnicolinear/2-4collinear.htm)
