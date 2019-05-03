---
id: 1324
title: Teams + Skype for Business
date: 2016-11-02T16:31:10+00:00
author: Richard Brynteson
layout: post
guid: http://masteringlync.com/?p=1324
permalink: /2016/11/02/teams-skype-for-business/
panels_data:
  - 'a:0:{}'
nkweb_code_in_head:
  - default
nkweb_Use_Custom_js:
  - default
nkweb_Custom_js:
  - ""
nkweb_Use_Custom_Values:
  - default
nkweb_Custom_Values:
  - ""
nkweb_Use_Custom:
  - 'false'
nkweb_Custom_Code:
  - ""
categories:
  - Microsoft Teams
---
The integration story between Teams and Skype for Business is an important one because the amount of overlap between the two products is surely going to cause all sorts of confusion among customers.  So having the two worlds talk to each other is going to be important.

The good news is, there is an integration story if you are using Skype for Business (SfB) Online.  The bad news is, if you are on-prem with SfB, there is no integration story and a few things are going to be less than ideal overall.  At the current time, the only integration between the two platforms is the ability to instant message between the products.

[EDIT: I didn&#8217;t mention it here before initially I did in my larger review you can see SfB Meetings under the meeting tab.  Intent here was to talk IM/P integration specifically.]

**Scenario #1 &#8211; Microsoft Teams user sending to Skype for Business User**

<a href="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/11/4-1.png" rel="attachment wp-att-1328"><img class="alignnone wp-image-1328" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/11/4-1.png?resize=550%2C418&#038;ssl=1" alt="4" width="550" height="418" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/11/4-1.png?w=878&ssl=1 878w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/11/4-1.png?resize=300%2C228&ssl=1 300w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/11/4-1.png?resize=768%2C584&ssl=1 768w" sizes="(max-width: 550px) 100vw, 550px" data-recalc-dims="1" /></a>

A few things I&#8217;ve notice about this workflow that are important to call out.

  * If the SfB Account for the Sender is hosted on-prem its going to fail with a bad error message.  As you can see in the screen shot, it will make it appear as though you can send the IM message at first but will just get stuck in a sending state forever.
  * Although the user is logged into and using Microsoft Teams they MUST be enabled for Skype for Business.  If the user isn&#8217;t enabled for Skype for Business, then nothing works.
  * SfB Presence is VERY SLOW to update and is misleading.

What does that last one mean?  When a Microsoft Teams user logs into the client it is also logging in via a customized version of UCWA to the Office 365 environment.  You can see this to a degree from the SfB Client.  If you are logged into a Skype for Business client as the receiver and the Teams user is not logged into SfB or Teams, they will appear offline.

<a href="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/11/3-1.png" rel="attachment wp-att-1327"><img class="alignnone wp-image-1327 size-full" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/11/3-1.png?resize=406%2C82&#038;ssl=1" alt="3" width="406" height="82" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/11/3-1.png?w=406&ssl=1 406w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/11/3-1.png?resize=300%2C61&ssl=1 300w" sizes="(max-width: 406px) 100vw, 406px" data-recalc-dims="1" /></a>

Once I have logged into my Teams client as my sender you will notice the status update for the user.

<a href="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/11/2-1.png" rel="attachment wp-att-1326"><img class="alignnone wp-image-1326 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/11/2-1.png?resize=402%2C82&#038;ssl=1" alt="2" width="402" height="82" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/11/2-1.png?w=402&ssl=1 402w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/11/2-1.png?resize=300%2C61&ssl=1 300w" sizes="(max-width: 402px) 100vw, 402px" data-recalc-dims="1" /></a>

When I log off of the Teams client, you will notice the presence doesn’t update and it appears as though I&#8217;m still online.

<a href="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/11/2-1.png" rel="attachment wp-att-1326"><img class="alignnone wp-image-1326 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/11/2-1.png?resize=402%2C82&#038;ssl=1" alt="2" width="402" height="82" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/11/2-1.png?w=402&ssl=1 402w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/11/2-1.png?resize=300%2C61&ssl=1 300w" sizes="(max-width: 402px) 100vw, 402px" data-recalc-dims="1" /></a>

I&#8217;ve tested it a few times, it will remain in this incorrect presence for a solid 10 minutes.  The user will go to Away after 5 minutes and then eventually to Offline.  It appears as though the log off event from Teams doesn&#8217;t trigger a corresponding log off event in SfB Online through that customized UCWA implementation.

Outside of the above item of note the IM/Presence integration works exactly as expected.  Users from Teams can start an IM and continue that conversation with a Skype for Business user.

**Scenario #2 &#8211; Skype for Business user sends message to a Teams/SfB User**

In this scenario it&#8217;s all about where the receiver (User B) is hosted and which clients the user will get the toaster pop in.  Essentially, if User B is hosted in SfB Online and logged into Teams at the same time, they will get dual notifications &#8211; one in SfB and one in Teams.  If the user is user is hosted on-prem for SfB then they will only get a toaster pop in SfB.

<a href="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/11/1-1.png" rel="attachment wp-att-1325"><img class="alignnone wp-image-1325" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/11/1-1.png?resize=550%2C392&#038;ssl=1" alt="1" width="550" height="392" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/11/1-1.png?w=646&ssl=1 646w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/11/1-1.png?resize=300%2C214&ssl=1 300w" sizes="(max-width: 550px) 100vw, 550px" data-recalc-dims="1" /></a>

Hopefully that takes a little bit of the mystery out of Teams + Skype for Business integration.