---
layout: post
title: "Hastings Wireless: An Introduction"
date: 2002-10-15T00:00:00-00:00
draft: false
author: "Brian Kloppenborg"
categories: ["hastings-wireless", "startups"]
tags: ["retrospective"]
---

## Introduction

Hastings Wireless is a group of computer and radio enthusiasts who are pushing
the limits of technology to the max! Our objective is to provide 802.11b
wireless networking in public spaces in the city of Hastings, NE. First, we
intend to create a backbone network using packet radios or long-distance
wireless links and then deploy small local access points (Nodes) using standard
consumer-grade 802.11b technology.

## Mission Statement and Development

It is the objective of Hastings Wireless to:

* Create a backbone network to allow participants and users to connect with
  their home internet connection throughout the city of Hastings, Nebraska and
  surrounding communities.
* Educate the citizens of Hastings and the surrounding community about the
  benefits and risks of wireless networking
* Provide a network in which new technologies and protocols may be
  developed.Provide information on the development and deployment of the
  project.

## Methods

The backbone for Hastings Wireless will be established using directional Packet
Radio Technology for which users MUST have a FCC licence to operate. The
current plan is to create a backbone that offers T1 like speeds (1.536 Mbps)
using a radio design created by Clint, KA70EI a radio enthusiast. Additional
information about this radio can be found at 
[http://www.ussc.com/~turner/t1modem.html](http://www.ussc.com/~turner/t1modem.html)


The backbone will connect to local node servers which will relay network traffic
using standard 802.11 wireless networking technology. Each server will be
required to run a caching server to further reduce the demand on the backbone.

Whenever possible 802.11 networking will be used in lieu of packet radio for
establishing directional links due to lower costs and time for construction and
deployment.

## The Future

If Hastings Wireless grows to the extent that is hoped, a larger network and
more options will become available. Commercial resale of bandwidth has been
among our considerations where we believe we can offer the masses a better
option for internet connectivity in rural areas. The founders of Hastings
Wireless believe that many of the local ISP's do not offer what we feel is a
fair market price for internet when significantly more bandwidth is offered for
the same price only a few miles away. It is for this reason that the development
of Hastings Wireless is a community effort. Hopefully, members from outside the
city of Hastings will participate and push their local ISP's to be competitive
with a community networking project.

## Subnets

Each node will be required to use a separate subnet within the 192.168.x.y
range. The value of "x" will be assigned by Hastings Wireless to prevent
networking conflicts and the values of "y" may be assigned via DHCP or static
assignments.
