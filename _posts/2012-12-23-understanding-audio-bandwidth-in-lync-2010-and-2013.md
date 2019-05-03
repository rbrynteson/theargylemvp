---
id: 111
title: Understanding Audio Bandwidth in Lync 2010 and 2013
date: 2012-12-23T14:59:24+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=111
permalink: /2012/12/23/understanding-audio-bandwidth-in-lync-2010-and-2013/
st_layout_box:
  - right
categories:
  - Lync Server 2010
  - Lync Server 2013
---
So this is one of those posts that I&#8217;ve been meaning to write for some time but never got around to do doing it, however with Lync Server 2013 coming out I thought it would be a good time to review exactly how to estimate bandwidth usage between sites.

Lync Server 2013 introduced some very large changes from the video aspect but to fully appreciate them I think we should take a moment to look back at Lync Server 2010.  In this post we will start with understanding how audio should be calculated.  You may want to take a moment to look at the TechNet article on bandwidth calculations.

<http://technet.microsoft.com/en-us/library/jj688118%28v=ocs.15%29.aspx>

Understanding what goes into bandwidth calculations requires you to understand the codec being used and the number of participants involved.  When you start introducing conferencing it&#8217;s gets even more complicated.  But without fully understanding what is involved, you are likely to make a calculation error along the way.  Microsoft has provided th bandwidth calculator but I always thought it was a little too black magic and people would randomly throw number in there without understanding exactly what was happening.  I also think that Microsoft believes every company has 30 sites and 100,000 users.

In this post we will look at several different examples of bandwidth usages and calculations.  My hope is to educate you in exactly what is happening.  I will assume throughout the article that Call Admission Control is not being used.

**Audio &#8211; Peer to Peer**

This is by far the easiest calculation to determine and fortunately this hasn&#8217;t changed from Lync 2010 to Lync 2013.  First off, keep in mind that traffic will be peer to peer and does not include the server in the media stream (unless media is turning off of the edge server for a remote to internal person for example).

On a Lync to Lync call, you will be using RT Audio WB, for the codec.  This is a dynamic codec which means calculation of the exact amount of bandwidth is impossible but we know the max that can be used is 62 Kbps.  Microsoft provides three numbers, typical, max and max with FEC (Forward Error Correction).  For planning, I always recommend to use the maximum number without FEC.  Because you cannot control when FEC will be used it isn&#8217;t the best standard to use for planning purposes.  Although typical is nice, this is the average and assumes a large call volume.  If we are dealing with tens of thousands of calls then it might make sense, but most companies don&#8217;t have that type of call volume and hence the max number without FEC makes the most sense.

A question that comes up often is, do I need to multiple my answer by two, and in a peer to peer call the answer is no.  On average the two individuals are not both screaming at the same time, therefore when one side is sending traffic the other side is listening and therefore is not sending any traffic.

So we know in our P2P call we should be consuming approximately 62 Kbps per call.  Of course, if this call is destined for the PSTN we will most likely be using G.711 from the client to the PSTN gateway if we have media bypass or potentially RT Audio NB as the codec.

To determine total amount of traffic being used multiply the simultaneous calls over the WAN * our codec and we have the amount.

**Audio &#8211; Conference**

We are now going to get a lot more complex in our examples.  First you need to know that the conferencing codec of choice is G.722 or Siren, the Lync 2010 and 2013 will default to G.722 first.  This codec is 101 kbps in size and all of that traffic is destined for the front-end server where the conference is being hosted.

So let&#8217;s look at a few examples of how we might calculate this.

Let&#8217;s say we have two offices, one in Minneapolis and one in Dallas, and both have Lync pools.  In Minneapolis we have 10 people in a conference and in Dallas we have another 10 people.  First we must determine who is hosting the conference so we need determine the direction of the bandwidth.   So let&#8217;s say a user in Minneapolis is the organizer of the conference.  Knowing that the AVMCU is located there, the Dallas to Minneapolis bandwidth used would be the 10 users x 101 kbps or 1010 Kbps.  This represents the individual media stream to each user in Dallas from the Minneapolis AVMCU.

But we need to keep in mind that someone in the Dallas office might speak so we need to calculate the Dallas to Minneapolis bandwidth also.  The easy way to think of it is that it would be 101 Kb.  Microsoft in their planning guides have a formula you can use as well.  They state that on average 80% of the time there is a single speaker during a conference, 7% of the time there are two speakers and 13% of the time is complete silence.  So your calculation would be something like:

(1 \* 101 \*.80) + (2 \* 101 \* .07) + (0 \* 101 \* .13) = 94.94

Now this number is not really different than the 101 but if we are trying to be precise it&#8217;s good to know.  So approximately 101 Kb is sent over the way from Dallas to Minneapolis.  So our total hit on the WAN is:

1010 + 94.94 = 1104.94 Kb or approximately 1Mb of traffic.

When estimating traffic make sure to think about all the possible directions that it might come and go.  Remember your external users to edge traffic, your AVMCU to gateway/PSTN traffic and then the remainder of your internal traffic.

**Summary**

Understanding how to calculate bandwidth will go a long way in helping you understand exactly what needs to be done for planning purposes.  Without a good understanding of the above it is very difficult to order circuits or implement call admission control.  Although you can blindly use the bandwidth calculator provided by Microsoft I think understanding some basics will help with that tool as well &#8211; although I typically never use it.

In the next couple of days we will dig into bandwidth calculations of Lync 2013 and its new video features including how to prepare for gallery view.