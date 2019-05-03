---
id: 1387
title: Planning for Shared Line Appearance in Skype for Business
date: 2017-06-16T20:52:41+00:00
author: Richard Brynteson
excerpt: 'The undocumented capacity planning guides for Microsoft Skype for Business Shared Line Appearance.  '
layout: post
guid: http://masteringlync.com/?p=1387
permalink: /2017/06/16/planning-for-shared-line-appearance-in-skype-for-business/
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
colormag_page_layout:
  - default_layout
image: https://masteringlync.com/wp-content/uploads/sites/2/2017/06/SLA.png
categories:
  - Lync Server 2013
  - Skype for Business
---
In Skype for Business Cumulative Update 1 (CU1), Microsoft introduced a new feature called Shared Line Appearance (SLA). The feature was so popular and requested that in a later CU, Microsoft back-ported the code to Lync 2013 for those who had not upgraded to SfB.  I won&#8217;t get into the configuration of <a href="http://masteringlync.com/2015/11/18/skype-for-business-cu1-shared-line-appearance/" target="_blank" rel="noopener">SLA </a>as you can check out this blog post for more details.

**What Is It Again?**

For those of you who may not know, SLA is a feature that brings old school telephony to the Microsoft Lync 2013 and Skype for Business.  What was that old school telephony?  Blinking lights.  For many people coming from a legacy telephony experience were used to being able to put a person on hold and then going to another phone to retrieve the call.

Today, Polycom VVX phones (and soon other phone providers) support SLA functionality on their phones.  When configured, you see multiple lines appears.

<img class="alignnone" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/11/4.png?resize=511%2C372&#038;ssl=1" alt="" width="511" height="372" data-recalc-dims="1" /> 

From the start I believed SLA was a game changer for Microsoft.  It allowed SfB to enter many new markets that once were not available, specifically retail.  We have found that SLA, due to it&#8217;s flexibility, works as a great replacement for Response Group Services (RGS) in many cases as well.  Most people who use RGS don&#8217;t use it for specific ring methods (serial, in order, attendant) but rather just to ring a handful of people &#8211; like the front-desk, human resources, etc.

**Limitations**

We were so excited about SLA we just started deploying it everywhere.  And I do mean everywhere.  In one of our deployments we have well over 1200 SLA&#8217;s deployed and actively being used on a daily basis.  With numbers comes lessons and here are a few.

<span style="text-decoration: underline">Capacity</span>

I don&#8217;t remember exactly the point where our servers started to crash but in hindsight I want to say it was around 500 SLA Groups on an Enterprise Pool (3 node).  We would wake up in the morning and find the RTCHost.exe service running wild.  Not CPU (although it did that as well) but memory.  We first realized the problem when SCOM told us that one of our front-end servers was almost out of memory.  A quick investigation found RTCHost.exe running at over 17 GB of memory usage!  We quickly planned for maintenance, rebooted the server and found within hours the problem was back.

I won&#8217;t get into all of the crazy memory dumps but a co-worker (<a href="http://blog.thepbxisdead.com/" target="_blank" rel="noopener">Mitch</a>) worked with Microsoft to discover the root of the problem.  I&#8217;ll let him tell the story because I find it interesting.  Bottom line, there is a bug in SLA and it causes some radical memory issues in Enterprise Servers when you get into huge numbers of SLA.  Standard isn&#8217;t impacted by this bug and it&#8217;s due to be fixed in the future (we hope) but it got us to ask the question: what are the limits for SLA?

After asking a few people at Microsoft I was finally able to get an answer of what was tested with SLA.

  * _200 SLA Groups per Enterprise Pool (8 FE&#8217;s)_
  * _Limit of 25 Delegates max per SLA group_
  * _Tested up to 16 lines per SLA group_

It is also worth noting that SLA is supported in pool pairing and failover.  I did ask if any specific testing was done with regards to Standard Edition Pools and there it gets a bit hazy.  Since SLA is done via signaling and not a presence subscribe it does seem to scale pretty good.  Remember, our RTCHost issue presented itself on a 3 node server with nearly 3 times the number of SLA Groups tested.  If we use some rough estimates, if 200 SLA Groups at 16 lines was good on an 8 node Enterprise one could argue that a single standard edition box would be fine with 25 SLA Groups at 16 lines.  Again, this is a bit of an extrapolation based on the tested Enterprise Numbers.

<span style="text-decoration: underline">Forwarding</span>

This one might be a bit unique and odd but it&#8217;s worth noting.  You can&#8217;t forward one SLA to another SLA if it has the same members in it.  Huh?  Exactly.

Let&#8217;s say SLA1 has User A, User B and User C in it.  And SLA2 has User A, User B and User D in it.  You could setup the no answer for SLA1 to forward to SLA2 in the configuration.   What happens is odd, User A and User B in SLA2 will not ring.  They get no toast on either the phone nor client.  Nothing.  If User D answers the call and puts it on hold, User A and User B can see the call and pick it up then.  But they are not offered the call initially.

Something within the SLA framework goes nuts as far as I can tell.  We found that if you bounced the call off another user or RGS then it&#8217;s fine.  For example, if SLA1 unanswered forwarded to RGS1 which has no members, no time of day routing and is setup to immediately forward to SLA2 then everyone gets rung.  It&#8217;s a bit of a crazy work around but it works.

Hopefully that helps.  More posts on SLA is coming soon.