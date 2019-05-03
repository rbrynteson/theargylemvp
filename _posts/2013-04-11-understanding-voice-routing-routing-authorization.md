---
id: 257
title: 'Understanding Voice Routing | Routing &amp; Authorization'
date: 2013-04-11T06:37:20+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=257
permalink: /2013/04/11/understanding-voice-routing-routing-authorization/
st_layout_box:
  - right
panels_data:
  - 'a:0:{}'
categories:
  - Lync Server 2013
tags:
  - Routing
---
In the previous post we covered the Dialing Behaviors of our outbound call to 952–555–1111.  In this post we will continue that investigation and focus on the Routing & Authorization of the call.  Make sure to read part one of this post for full details.

  * Investigating Dialing – [Dialing Behaviors](http://masteringlync.com/2013/04/07/understanding-voice-routing-dialing-behaviors/)
  * Investigating Dialing – [Routing & Authorization](http://masteringlync.com/2013/04/11/understanding-voice-routing-routing-authorization/) &#8211; This Post
  * Investigating Dialing – [Dialing Behaviors of an Inbound Call](http://masteringlync.com/2013/05/31/understanding-voice-routing-inbound-routing/) 
  * Investigating Dialing – [Inter Trunk Dialing](http://masteringlync.com/2013/06/07/inter-trunk-routing-deep-dive/)

**Vacant Number Range**

Many people are surprised to learn that all inbound and outbound calls check the vacant number range.  So when our call goes out to +19525551111 we see that we don&#8217;t match any numbers in the vacant number range and take no action.

[<img class="alignnone wp-image-1026 size-medium" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/04/voice11_thumb1-300x35.jpg?resize=300%2C35&#038;ssl=1" alt="voice11_thumb" width="300" height="35" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/04/voice11_thumb1.jpg?resize=300%2C35&ssl=1 300w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/04/voice11_thumb1.jpg?w=553&ssl=1 553w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/04/voice11_thumb1.jpg)

&nbsp;

[<img class="alignnone wp-image-1027 size-medium" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/04/voice12_thumb1-300x38.jpg?resize=300%2C38&#038;ssl=1" alt="voice12_thumb" width="300" height="38" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/04/voice12_thumb1.jpg?resize=300%2C38&ssl=1 300w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/04/voice12_thumb1.jpg?w=552&ssl=1 552w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/04/voice12_thumb1.jpg)

However, since all outbound calls are flowing through this number range you can use the vacant number range for all sorts of interesting routing decisions.  For example, maybe you have historically used a route like this to prevent unauthorized dialing of 900 and 967 numbers:

^+1(?!(900|976))[2-9]d{2}[2-9]d{6}$

However, this process has one draw back, it blocks route matching and makes tracking those who are trying to make this call difficult.  So instead, you could use the Vacant Number Range of +19000000000 to +19009999999 to capture all calls to this service and route them to an announcement service which then routes to the Human Resource department for example.  Basically, you can have all sorts of fun for blocking internal users from dialing unwanted numbers.

Since we don&#8217;t match the vacant number range we move onto the next step.

**Call Park Range**

This check is interesting as you could potentially do this check twice depending on the call flow although in this trace the first check was skipped due to the global check.  Like the vacant number range we could look in the same log and see if our number matches any Call Park information.

[<img class="alignnone wp-image-1028 size-medium" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/04/voice13_thumb11-300x41.jpg?resize=300%2C41&#038;ssl=1" alt="voice13_thumb1" width="300" height="41" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/04/voice13_thumb11.jpg?resize=300%2C41&ssl=1 300w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/04/voice13_thumb11.jpg?w=551&ssl=1 551w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/04/voice13_thumb11.jpg)

[<img class="alignnone wp-image-1029 size-medium" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/04/voice14_thumb1-300x59.jpg?resize=300%2C59&#038;ssl=1" alt="voice14_thumb" width="300" height="59" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/04/voice14_thumb1.jpg?resize=300%2C59&ssl=1 300w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/04/voice14_thumb1.jpg?w=549&ssl=1 549w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/04/voice14_thumb1.jpg)

Here we can see there were no matches.

Have you wondered how the new Group Pickup Feature (introduced in February 2013: CU1 Patch for Lync Server 2013) works.  They accomplish the group pickup by simultaneously ringing your end point and the call park service at the same time.  So if your group pickup range was #100 to #149, you may have assigned #100 to User A and when that call arrives you can see the call sim ring into the call park service.  It&#8217;s an interesting approach Microsoft decided to use here without having to build a new service for this one feature.

There are some very interesting implications of Call Park on incoming calls as well but we will save that for our next post.

**Voice Policy, PSTN Usages and Routes**

We have finally arrived at the actual routing and authorization of the call.  It may feel like this process has taken a long time to get here but rest assured this is all done so fast the end user is unaware.  Its first important to remember what a Voice Policy is, it&#8217;s a collection of PSTN Usages that have been assigned to the user.  These PSTN usages are then connected to routes and that determines what the user is allowed to route to.

Although the Voice Policy and Routes are separate boxes on this slide it is important to remember that one does not work without the other.  So it&#8217;s important to investigate these together.  What happens is all routes are gathered in order based on the order of the PSTN Usages in the Voice Policy and then looped through to determine if a match exists.  So when our outbound call for 952–555–1111 has reached this far, we can see in the OutboundRouting log a match to a PSTN Usage:

[<img class="alignnone wp-image-1030 size-medium" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/04/voice16_thumb1-300x47.jpg?resize=300%2C47&#038;ssl=1" alt="voice16_thumb" width="300" height="47" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/04/voice16_thumb1.jpg?resize=300%2C47&ssl=1 300w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/04/voice16_thumb1.jpg?w=551&ssl=1 551w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/04/voice16_thumb1.jpg)

Which matches to this route:

[<img class="alignnone wp-image-1031 size-medium" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/04/voice17_thumb1-300x62.jpg?resize=300%2C62&#038;ssl=1" alt="voice17_thumb" width="300" height="62" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/04/voice17_thumb1.jpg?resize=300%2C62&ssl=1 300w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/04/voice17_thumb1.jpg?w=552&ssl=1 552w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/04/voice17_thumb1.jpg)

It&#8217;s a small detail but it&#8217;s important to remember from a call flow perspective it&#8217;s a failure to match a PSTN usage that create the 403: No Route Found message.  But our call found a matching usage and route so now it&#8217;s time move to the next step.

**Trunk Configuration**

Other than the reference to Group Call Pickup (part of CU1 for Lync Server 2013) everything up to this point would be true for both Lync Server 2010 and Lync Server 2013. In Lync Server 2013 we have the option to modify both the called and calling number (in 2010 it was only the called number).  This makes it very easy for use to move much of the transformation rules that once existed on the IP Gateway to Lync creating a single place for all normalization rules.  This is my preferred approach so I can make my gateway deployments a set-it and forget-it deployment.

So here we have a simple set of trunk rules.  Here we move the +1 from our calling number (our caller ID) and two simple trunk rules in this example:

[<img class="alignnone wp-image-1032 size-medium" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/04/voice18_thumb1-300x35.jpg?resize=300%2C35&#038;ssl=1" alt="voice18_thumb" width="300" height="35" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/04/voice18_thumb1.jpg?resize=300%2C35&ssl=1 300w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/04/voice18_thumb1.jpg?w=551&ssl=1 551w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/04/voice18_thumb1.jpg)

In this trace you can see that I&#8217;m matching the remove rule to strip my E164 number down to only seven digits and my calling number is stripped down to only 10 digits.

**Success**

And now the call reaches the end for Lync and is routed out to the IP Gateway/SIP Provider/Legacy PBX (who uses those anymore?).  All of this tracing was done via OCSLogger or CLSLogging and all you need is Outbound Routing and TranslationApplication and use Snooper to view.  Thur far we have discussed only an Outbound Call but in our next section we will discuss Inbound Call routing from the PSTN.