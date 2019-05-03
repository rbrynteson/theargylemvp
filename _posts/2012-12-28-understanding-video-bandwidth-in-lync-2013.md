---
id: 114
title: Understanding Video Bandwidth in Lync 2013
date: 2012-12-28T20:28:02+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=114
permalink: /2012/12/28/understanding-video-bandwidth-in-lync-2013/
st_layout_box:
  - right
categories:
  - Lync Server 2013
---
In part one we looked at how to determine [Audio Bandwidth in Lync Server 2010 and 2013](http://masteringlync.com/2012/12/23/understanding-audio-bandwidth-in-lync-2010-and-2013/).  In this post we will dig into how to determine how much bandwidth will be consumed with video in the new Lync Server 2013.  In Lync Server 2010 this process was pretty straight forward.  If we look back to 2010 because of RTVideo codec we had potentially two resolutions that would be used &#8211; VGA and CIF.  They used a maximum of 610 Kbps and 260 Kbps respectfully.  If you were doing Peer to Peer, we also had the potential of HD Video but only if a person went full screen with the video feed.  Calculating video bandwidth was as straight forward as the previous article on calculating audio traffic.

In Lync 2013, things get a lot more complicated and hopefully this post will give you some background on why that’s true.

The first reason is the video codec change.  The move from RTVideo to H.264 completely changes how we determine the amount of bandwidth that will be consumed and used.  Remember this table from the previous post:

<http://technet.microsoft.com/en-us/library/jj688118%28v=ocs.15%29.aspx>

He were see we have a potential of eight different video resolutions (and three panoramic) and therefore eight potential maximum bandwidth that could be consumed.

The second reason is the addition of the gallery view (standing and sitting row).  Here, we have the potential to have up to five active video streams in the client plus a single panoramic view.  This means as a consumer of bandwidth the Lync 2013 client has the potential to consume a lot more.  Additionally, the client itself can send up to five video streams as well.

Here are the important part of the above TechNet Article:

<p style="padding-left: 30px">
  <em>If video is used, all participants can receive up to five receive video streams and one panoramic (for example, aspect ratio 20:3) video stream. By default the five receive video streams are based on active speaker history but users can also manually select the participants from which they want to receive a video stream.</em>
</p>

<p style="padding-left: 30px">
  <em>Each participant that turns on the user’s send video stream will send one or more video streams. Lync 2013 add the capability of sending up to five video streams to optimize the video quality for all the receiving clients. The actual number of video streams being sent is determined by the sender based on CPU capability, available uplink bandwidth, and the number of receiving clients requesting a certain video stream. The most common case is that one H.264 and one RTVideo video stream are being sent in case a legacy client joins the conference. Another common scenario is that several H.264 video streams (for example, with different video resolutions) are sent to accommodate different receiver requests.</em>
</p>

So let&#8217;s look at an example.  What if my Lync Client looks like this:

<a href="http://masteringlync.com/2012/12/28/understanding-video-bandwidth-in-lync-2013/6710-gallerypic14/" rel="attachment wp-att-116"><img class="alignnone size-full wp-image-116" alt="6710.gallerypic14" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2012/12/6710.gallerypic14.png?resize=550%2C327&#038;ssl=1" width="550" height="327" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2012/12/6710.gallerypic14.png?w=550&ssl=1 550w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2012/12/6710.gallerypic14.png?resize=300%2C178&ssl=1 300w" sizes="(max-width: 550px) 100vw, 550px" data-recalc-dims="1" /></a>

In this example I&#8217;m actively consuming 5 video streams.  Now, the trick is to determine what bandwidth is actually being consumed.  Here is where the estimation is going to start.  When you start the gallery view, the images of each person is fairly small and most likely at 212&#215;160 which means each feed is going to consume a maximum of 250 Kbps or approximately 1250 Kbps.  So in addition to me consuming that much, I will also be providing my video stream as well so there would be another 250 Kbps.  So we break it down as:

AV/MCU to Client &#8211; 1250 Kbps

Client to AV/MCU &#8211; 250 Kbps

Total: 1500 Kbps

Keep in mind these are the maximum amounts.  At this resolution I could consume/send as little as 15 Kbps per stream.  Most likely, it won&#8217;t be this low unless we are constrained for bandwidth for some reason but there is the possibility.

Now this is the simplest possible example.

Let&#8217;s examine the next scenario.

<a href="http://masteringlync.com/2012/12/28/understanding-video-bandwidth-in-lync-2013/4848-gallerypic5/" rel="attachment wp-att-115"><img class="alignnone size-full wp-image-115" alt="4848.gallerypic5" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2012/12/4848.gallerypic5.png?resize=550%2C361&#038;ssl=1" width="550" height="361" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2012/12/4848.gallerypic5.png?w=550&ssl=1 550w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2012/12/4848.gallerypic5.png?resize=300%2C197&ssl=1 300w" sizes="(max-width: 550px) 100vw, 550px" data-recalc-dims="1" /></a>

Here we have &#8220;popped&#8221; out the video feed and now I can resize my window and create larger thumbnails in the gallery view.  When we do this we have the potential for multiple sizes and multiple streams.

So let&#8217;s go back to my example from above.  Let&#8217;s pretend our conference has 10 people and no one has done anything with the size of their video/gallery so each of those 10 people are consuming 1250 Kbps and sending 250 Kbps.  Now three people have popped out their video and resized the window so they are no longer at 212&#215;160 but now at 424&#215;320.  So these users will now consume five video streams at a maximum of 450 Kbps or 2250 Kbps.  Additionally, all clients are going to need to send both the lower 212&#215;160 resolution for the seven people and now the higher 424&#215;320 resolution for the three people &#8211; so 250 Kbps + 450 Kbps or another 700 Kbps.

So now all ten clients are sending both the lower and higher resolution stream or 700 Kbps.  Seven people are downloading five streams at the lower 1250 Kbps (5 x 250 Kbps) and three people are download five streams at the higher 2250 Kbps (5 x 450 Kbps).

As you can see this gets really complicated.  What if one of those three people went from 424&#215;320 to 960&#215;540, we would then suddenly have three streams in play.

So how do we estimate bandwidth?  First, it&#8217;s important to remember that all of these are the maximum numbers.  So we could potentially use something like Call Admission Control or some new conferencing policies to force a lower amount.  Second, there is a maximum number of streams and the lowest common denominator always wins.  So if you were in a situation where there were many video streams at play and a sixth lower resolution was introduced all of the clients would need to start sending that lower stream and some at the higher level would need to negotiate to a lower resolution.  Third, according to sources at Microsoft, the average number of streams is typically less than two per client based on testing.  This of course will vary based on your business needs but should reassure you that simple because we can send up to five streams doesn’t mean it&#8217;s going to happen all the time.  Fourth, if you need to, you can always disable the multiple video streams based on the conferencing policy of the user/site/pool/global.

Hopefully this information will help you understand how to plan for the new conferencing features of Lync Server 2013.  In the next post we will discuss how you can control bandwidth in your environment.