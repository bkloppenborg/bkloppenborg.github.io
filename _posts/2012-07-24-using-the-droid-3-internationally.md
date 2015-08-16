---
layout: post
title: "Using the Droid 3 internationally."
description: "Instructions on how to configure the Droid 3 for use on non-US SIM networks."
category: hardware
tags: [Droid3, international travel]
---
{% include JB/setup %}

One of the most important things for geeks is keeping in constant communication.  That means email, internet, SMS, etc.  Fortunately my cellphone (Droid 3) is a CDMA and GSM device.  With Verizon you have two options, you can either pay for their international data service on Vodaphone's network, or you can pick up a SIM card with data when you arrive at your destination.  For me, the second option was significantly less expensive.

Below are some instructions on getting your Droid 3 to work on a GSM network.  I can't guarentee they work everywhere, but it did for me (on O2 in Germany).

## Before you leave:

1. Call Verizon Global support and ask them for the SIM unlock code.  This will be an 8-digit number you will have to enter the first time you insert a new SIM card into your phone.
2. Find someone on AT&T or T-mobile and ask to borrow their SIM for unlocking.
3. Power off your phone
4. Remove the battery cover, battery, and the Vodaphone SIM
5. Insert their SIM, and reinstall the battery
6. Startup the phone.
7. Type in the SIM unlock code from Verizon.

Congratulations, your phone is now SIM unlocked. If this doesn't work, call Verizon Global or visit the local Verizon store for assistance.

**NOTE**: Verizon locks out all US-based GSM carriers, so you won't be able to actually use the US-carrier SIM card in your Droid3, just unlock it for use with foreign carrier SIM cards.

## After you arrive:

Once you are at your foreign destination there are a few things to consider.  If you want voice only, you only need to find a carrier that uses one of GSM 850/900/1800/1900.  If you want high-speed (3G) data, you need to select a carrier that uses one of  UTMS 850/1900/2100 (also known as B5/B2/B1 respectively).  After you decide what you want, simply purchase a SIM card from the carrier, activate it, and install the card in your phone.  If you have never activated a SIM card before, I would highly suggest getting the carrier to do it for you.

After a few minutes (to hours) the SIM card should be activated on the carrier's network.  When you turn on your phone, you will be prompted for the SIM's PIN number (normally found in the packaging that came with your SIM card).  Type in the number and you should be able to make/receive phone calls.

To get data working requires a little additional effort.  All we need to do is configure the Access Point Name (APN) for your carrier.
1. From the main screen, select Menu, Settings, Wireless Networks, Mobile Networks.
2. In this dialog, set the Network Mode to GSM/UMTS
3. Then choose "Access Point Names" (APN).  The resulting screen will be blank if you have never configured an APN before.
4. Choose menu, and select "New APN"
5. Do a quick Google search for your carrier's name in conjunction with "APN".  For O2, I found this page by searching for "o2 apn deutschland" on Google.
6. Type in the required information
7. Once done, press Menu and choose "Save"
8. Now, from the APNs dialog, check the button (far right) for the network you just created.

Within a few moments of doing this, your phone should automatically connect to the 3G network (it may require a restart or airplane mode on/off to do so though).

