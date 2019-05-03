---
id: 133
title: Controlling Video Bandwidth
date: 2013-02-05T23:51:32+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=133
permalink: /2013/02/05/controlling_video_bandwidth/
st_layout_box:
  - right
panels_data:
  - 'a:0:{}'
categories:
  - Lync Server 2013
---
In the previous two sections we looked at both audio and video bandwidth that can be consumed when using Lync 2013.      What we learned is that audio bandwidth hasn&#8217;t changed between Lync 2010 and 2013 but video bandwidth on the otherhand has completely changed and needs to be discussed in any deployment of Lync Server 2013.

Lync gives you two options to control bandwidth when designing your Lync 2013 &#8211; Call Admission Control (CAC) and Conferencing Policies.

I won&#8217;t go too deep into how CAC works but you should know that when assigning Audio and Video session limits in your CAC policy they are of course a cumulative value and not based on an individual steam.  So if your CAC policy is to limit to 500 Kbps per session that is all the user will get to support as many video streams as they consume or produce.  I note this not because it&#8217;s a bad thing just that CAC will do exactly what you expect it to do.

The second option is conferencing policies.  There are several new options that allow you to dictate exactly how much bandwidth can be consumed/sent and the behavior of the conference.

**Enable participants to join with multiple video streams**.  This options when enabled allows the client to display the gallery view including the ability to see five active video streams as a given time.  When this option is disabled, it reverts back to how Lync 2010 behaved and only shows a single active video stream at any given time.  Essentially this limits the amount of bandwidth that can be received.

<a href="http://masteringlync.com/2013/02/05/133/limitcontrol/" rel="attachment wp-att-134"><img class="alignnone wp-image-134 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/02/limitcontrol.png?resize=346%2C176&#038;ssl=1" alt="limitcontrol" width="346" height="176" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/02/limitcontrol.png?w=346&ssl=1 346w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/02/limitcontrol.png?resize=300%2C153&ssl=1 300w" sizes="(max-width: 346px) 100vw, 346px" data-recalc-dims="1" /></a> <a href="http://masteringlync.com/2013/02/05/133/limitcontrol2/" rel="attachment wp-att-135"><img class="alignnone wp-image-135 size-full" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/02/limitcontrol2.png?resize=370%2C152&#038;ssl=1" alt="limitcontrol2" width="370" height="152" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/02/limitcontrol2.png?w=370&ssl=1 370w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/02/limitcontrol2.png?resize=300%2C123&ssl=1 300w" sizes="(max-width: 370px) 100vw, 370px" data-recalc-dims="1" /></a>

**Allow Multiple Video Streams**.  This option instructs the client on if it should be allowed to send multiple streams for each video resolution requested as part of a conference.  Keep in mind that a client can send up to five streams of video and that the average client will send two streams as part of a client &#8211; this average is what was found during the TAP process and of course your environment may be very different.  This limits the amount of bandwidth that can be sent.

The next two options are available only via PowerShell.

**TotalReceiveVideoBitRateKb** &#8211; The max video bandwidth policy allows you to define the total amount of bandwidth a single users is allow to consume.  This can be very helpful if the user is connected on the far end of a low bandwidth link and you want to make sure a single person can&#8217;t consume the entire network.

<a href="http://masteringlync.com/2013/02/05/133/powershell/" rel="attachment wp-att-136"><img class="alignnone wp-image-136 size-full" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/02/powershell.png?resize=643%2C56&#038;ssl=1" alt="powershell" width="643" height="56" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/02/powershell.png?w=643&ssl=1 643w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/02/powershell.png?resize=300%2C26&ssl=1 300w" sizes="(max-width: 643px) 100vw, 643px" data-recalc-dims="1" /></a>

**VideoBitRateKb** &#8211; This setting allows you to essentially control the max resolution allow by Lync.  For example, if you set a person to a max bit rate of 400kbps, this would prevent all HD resolutions.  This has some advantages over CAC as it allows me to control bandwidth beyond a single value for the entire site. For example a CAC value of 400kbps might be good for all of employees but you need to allow a single conferencing room to have a value of 5000kbps because you want to allow HD from that system.  This scenario would be difficult to design using CAC exclusively unless you carved out the conferencing room to its own subnet.  Of course, a roaming person would be the disadvantage.  If a home office employee who has no limits visits an office with low bandwidth they may accidently consume all network traffic.

In most deployments a combination of CAC and conferencing policies should be used.  Lastly it should be noted that each of these values are defaulted at 50000 kbps, essentially allowing a user to do anything they want.