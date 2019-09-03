---
title: Last Week (in Teams) This Morning - September 2nd
date: 2019-09-02T09:45:39+00:00
author: Richard Brynteson
layout: post
permalink: /2019/08/14/where-did-my-chat-disappear-to/
categories:
  - Microsoft Teams
  - Skype for Business
---

A LONG time ago I decided I was going to try to post weekly updates to my blog.  Not only with what has changed in the Microsoft Teams and Skype for Business community but also with some commentary that may or may not laced with sarcasm.  I was good about it for about a month and then it fell off.  So here is my attempt again to fix that.  The plan is to have a new post every Monday morning so while you enjoy your cup of joe you have something to read.
So without any more introductions:

<img src="https://theargylemvp.com/assets/images/lwtm.png" />

## Meetings First Mode

As seen all over Twitter and the [Tech Community]( https://techcommunity.microsoft.com/t5/Microsoft-Teams-Blog/Meetings-First-brings-Microsoft-Teams-meetings-to-Skype-for/ba-p/828340) website was the big announcement of Meetings first mode.  Although I am super excited to see these announcements as being public this functionality has been baked for some time.  If you were a reader you would have known all about this from these handy blogs:

- 6 25 2019 – [Modes Are The Answer](https://theargylemvp.com/2019/06/24/modes-are-the-answer/)
- 6 24 2019 – [Why Have You Moved To Teams](https://theargylemvp.com/2019/06/24/why-not-move/) 

<img src="https://theargylemvp.com/assets/images/09032019-2.png" width="650" />

This is not a contest to who posted it first (I did :-) ) but instead it is exciting to see Microsoft finally get the word out.  I have been a big fan of meeting modes and have been running in this mode for some time.

But what is the general public saying about Meetings First mode (or all of the modes)?  The reaction has been mixed to say the least.  The biggest problem with the Modes is that your users are most likely in Islands mode and given that Microsoft made sure the ENTIRE world knew about all of the cool features of Teams it means to get back into one of the two usable SfB + Teams mode would require you to take something away from users.  In particular: Chat.

This might be an absolute showstopper for some users and customers.  But if you are in Skype for Business On-Premises and plan on remaining in the SfB World for quite some time due to Enterprise Voice and the complexity of it and missing features in Microsoft Teams then Meetings First is the answer and long term…will be best for your organization.

## It Is Time for a Hot Fix

Lets call this a REDO CU for Microsoft.  Just a few weeks ago Skype for Business Server 2015 CU10 rolled out to the world.  And if you were one of those lucky people who install production updates on your PBX really fast two notes:

1. Why in the world would you do this?
2. As you know, Poly VVX phones are broken with transfers.
So to fix this issue we now have [Hotfix 1](https://support.microsoft.com/en-us/help/3061064/updates-for-skype-for-business-server-2015) for SfB 2015 CU10.  

The one item to know is not only does the Hotfix resolve the above issue.  It was two other things.

1. It fixes a Skype Meeting issue
2. It removes a feature.  As noted near the bottom of the KB article it states, “The fix for the following issue is reverted and it is not included in this cumulative update.”  That reads to me they removed a fix to fix a problem.  I am guessing Microsoft found the root cause of their transfer issue potentially.

## What Is New In Teams

Each month the [Tech Community](https://techcommunity.microsoft.com/t5/Microsoft-Teams-Blog/What-s-New-in-Microsoft-Teams-August-2019/ba-p/830349) site is updated with what is new in Microsoft Teams.  And I have to admit it is really cool to have all of this document in the modern world we live.  In the old days we left this to MVPs to dig through the product and blog about the cool things they found.  

### Focus Time

This is really a feature of MyAnalytics that explores your work patterns and based on your calendar will tell you might want to schedule focused time in your calendar.  This is really cool but can we talk about what causes less focus in Teams.

Moving notifications of Teams from the app into the Action Center on Windows 10.  I am not sure who wanted this change but put me on the list of people who did not.  I am not sure about you but the first thing I do on Windows 10 is turn off all notifications because it OVER notifies all the time.  But with Teams in the action center I have to turn on notifications for Windows again and it is driving me insane.

### Magic Whiteboard

That is not the name but I just like the sound of it.  This is the feature that was showed off at Enterprise Connect where you could walk in front of a whiteboard that had a content camera configured.  And you could see through the person.

<img src="https://theargylemvp.com/assets/images/09032019-4.png" width="650" />

This is pretty cool!

## Templates

There are a few cool new App Templates available to Microsoft Teams.  I am not going to explain what they do because Tom Morgan has already done that work.

1. [App Templates – What Are They](https://blog.thoughtstuff.co.uk/2019/06/app-templates-for-microsoft-teams-what-are-they-why-do-you-care-and-why-should-everyone-be-using-them/)
2. [Two New App Templates](https://blog.thoughtstuff.co.uk/2019/08/2-new-microsoft-teams-app-templates-to-help-you-communicate-better-get-questions-answered/)

If you are not following Tom you should be.  His stuff is always amazing.

## Do You Use ConfigMgr

Well I do not use it but if I did use it there now a feature to integrate ConfigMgr and Teams together. 

<img src="https://theargylemvp.com/assets/images/09032019-3.png" width="650" />

So now you can directly start a chat with a user from ConfigMgr.  This is neat feature unless you are using Skype for Business + Teams in Meetings First mode above.

There you have it.  Ep 1 of season 1 is in the books.  And no I did not miss my Monday morning plan.  Yesterday was Labor Day in the United States so we were all off on holiday.  Hopefully you find some of this information useful.  And hopefully I can keep up this pace.  Lets see if I can do two weeks in a row.
