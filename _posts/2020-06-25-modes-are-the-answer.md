---
title: Modes are the answer!
date: 2019-06-25T14:37:39+00:00
author: Richard Brynteson
layout: post
permalink: /2019/06/24/modes-are-the-answer/
categories:
  - Microsoft Teams
  - Skype for Business
---

In my [previous post](https://theargylemvp.com/2019/06/24/why-not-move/) I recounted the reasons an organization might be holding onto Skype for Business and not moving to Teams for voice.   As we all know, voice is a very personal decision and an organization might take time to make a decision.  But in the meantime, if SfB is your Enterprise Voice solution than Teams should ABSOLUTELY be your Meetings and Collaboration solution.  There are really two reasons why you might hold onto SfB for Meetings.

1. Bandwidth.  Moving to Teams means you have to send all of your conference data across the internet.  If you are not provisioned or cannot get enough bandwidth, than Teams may not be the best solution.
2. Cost.  If you are a heavy PSTN Conferencing shop and only have E3 licenses, the costs of going to an E5 or adding PSTN Conferencing might be too much.

But if you are an E5 customer and bandwidth isn't your concern, than Meetings First is your solution to your meetings problem.

## The Modes

The [docs site](https://docs.microsoft.com/en-us/microsoftteams/upgrade-and-coexistence-of-skypeforbusiness-and-teams) has a great article on the different modes of Teams.  Although they have been around for a while, they were recently exposed in the Modern Portal and enforcement of them only started in March.  Of particular interest is the Skype for Business with Teams with Meetings and Collaboration.

![alt text](https://theargylemvp.com/assets/images/modes-image-1.png "Modes" =450x)

## What Is It

At a base level it's three things.

You keep running Skype for Business Server for chat, calling and presence.  

Meet in Teams.  This provides the best, richest, complete meetings experience including meeting rooms, devices, services (Cloud Video Interoperability).

Enjoy harmonious coexistence.  Your Teams and Skype for Business clients play nice with each other.  Additionally you get the full IT story – provisioning, reporting and more.

## Benefits

The meetings first experience provides a handful of excellent options.  

### Hold Incoming

When you enable Meetings First mode and have an updated Teams and SfB Client you get device interop between the products.  So what does that mean?  When you are in a Teams Meeting and a call comes in via Skype for Business and you answer it, the meeting in Teams goes on-hold.  This prevents the dreaded two devices, both on at the same time experience you might have in Islands mode.

### Devices Work Better Together

Again, this mode + updated client/Teams and you gain full control over your HID devices.  This means that when you click on mute or answer a call, the headset sends the commands to the correct client.  This reduces confusion or that moment when you thought you were muted because the headset said you were - but in reality you were not.

### Presence

This one is big.  Although it hasn't made it out to the production ring yet, Microsoft has detailed in great length that Meetings first includes a presence reconciliation process.  So when you are in a meeting in Teams, your SfB Client will show "In a Conference Call".  Do a desktop share in Teams, and if your client is set for it, you will go to Do Not Disturb.

The key item to know is that these modes are the best experience compared to any third party meeting solution.  For example, if you are in a Zoom meeting, the SfB client has no knowledge of this meeting taking place.  So when a call comes in and the user accidently answers it, they will get that scenario where their one headset is not connected to both calls at the same time.  The user will never know who they are actively talking too or who can hear what in the call.

