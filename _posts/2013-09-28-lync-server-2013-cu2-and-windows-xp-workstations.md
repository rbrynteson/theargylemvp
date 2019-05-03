---
id: 406
title: Lync Server 2013 CU2 and Windows XP Workstations
date: 2013-09-28T02:51:01+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=406
permalink: /2013/09/28/lync-server-2013-cu2-and-windows-xp-workstations/
st_layout_box:
  - right
panels_data:
  - 'a:0:{}'
categories:
  - Lync Server 2013
---
Microsoft has been on this new cadence of releasing patches that include both bug fixes and new features.  Every once in a while a CU is released that has so much stuff in it a feature gets overlooked in the rapid blogging of new features and here is one of them.  This past week I&#8217;ve been digging into the features (or lack there of) of using Windows XP and Lync Server 2013.  Granted, this is an OS that is over a decade old but there are still over 30% of PC&#8217;s running the OS so it&#8217;s difficult to ignore them.  When you get into the large enterprise its more likely you will find more than one WinXP workstation.  So let&#8217;s review what is and isn&#8217;t available:

<span style="text-decoration: underline"><strong>Full Client</strong></span>

The Office 2013 installer only works with Windows 7 and Windows 8.  So the only option for a full client is to deploy the Lync 2010 client to the workstation.  Good news, the 2010 client was rock solid, but it&#8217;s important to remember that there are feature differences.  But of course, this only works once the user is migrated from OCS 2007 R2/2010 to the new Lync 2013 environment.  If you have a small deployment this can be done fairly quickly but if you have a large global enterprise deployment migrating thousands of users will take some time which means co-existence between OCS 2007 R2 users and 2013 is a necessity.

<span style="text-decoration: underline"><strong>Lync Meetings</strong></span>

If you are an organization currently using OCS 2007 R2 than your Windows XP workstations are happily running the Live Meeting client for the full Audio/Video/Presentation experience.  If you were migrating to Lync Server 2010, than your only option was to deploy the Lync Attendee client to workstations during the migration to ensure your OCS users could continue to function.  This client was supported on Windows XP to Windows 7.

This was replaced in Lync 2013 with the Lync Web App (LWA) &#8211; a full featured experience that supported Audio/Video/Presentation within the browser without the need of an additional client.  This works great for Windows 7 and Windows 8 users using a variety of browsers.  However if you look at the fine print of the Lync Web App Supported Platforms guide you will find note #3.

_http://technet.microsoft.com/en-us/library/gg425820.aspx_

_On supported Windows XP, Windows Vista, and Windows Server 2008 operating systems, computer-based voice and video are not available. Application viewing, application sharing, desktop viewing, and desktop sharing are available._

With these requirements your Windows XP users are suddenly left without any means of using Audio and Video within the conference.  So what are your options?

**Dial-In Conference Bridge for All Users**

The first and most obvious option is to deploy the dial-in conference bridge feature of Lync Server 2013.  The steps to do this is well beyond the scope of this blog post but the end result is your Windows XP users would be able to join the presentation via Windows XP and Internet Explorer 8 and access the audio portion of the conference via the dial-in bridge.  However, in this solution there are no means to join the video portion of the presentation.

**Lync Attendee Client for IE6/7**<span style="text-decoration: underline"><strong><br /> </strong></span>

One of the new features of Lync Server 2013 CU2 was a fix for Windows XP users who use Internet Explorer 6 or IE7.  As we find in this KB article:

_[<span style="color: #0563c1">http://support.microsoft.com/kb/2853996</span>](http://support.microsoft.com/kb/2853996)_

_After users install this cumulative update, they are prompted to install Microsoft Lync Attendee when they try to join the meeting. If Lync Attendee is already installed, Internet Explorer should start Lync Attendee automatically._

Although the KB article mentions the users installing the CU this is actually a server side patch.  Once the web services patch has been applied when an IE6/IE7 user joins a meeting they will be presented with a new option.

<a href="http://masteringlync.com/2013/09/28/lync-server-2013-cu2-and-windows-xp-workstations/pic1-11/" rel="attachment wp-att-407"><img class="alignnone wp-image-407 size-medium" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/09/pic12-300x195.png?resize=300%2C195&#038;ssl=1" alt="pic1" width="300" height="195" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/09/pic12.png?resize=300%2C195&ssl=1 300w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/09/pic12.png?resize=768%2C499&ssl=1 768w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/09/pic12.png?w=921&ssl=1 921w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" /></a>

This will direct the user to download the User-Level install of the Lync 2010 Attendee client.  (NOTE: This installer requires the Windows Installer 3.1 and Windows XP SP3).

Once you have installed the Lync 2010 Attendee client you need to launch your meeting again via IE 6/7 to get the option to open the meeting via the Lync Attendee client.

<a href="http://masteringlync.com/2013/09/28/lync-server-2013-cu2-and-windows-xp-workstations/pic2-8/" rel="attachment wp-att-413"><img class="alignnone size-full wp-image-413" src="https://i2.wp.com/masteringlync.com/files/2013/09/pic21.png?resize=443%2C206&#038;ssl=1" alt="pic2" width="443" height="206" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/09/pic21.png?w=443&ssl=1 443w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/09/pic21.png?resize=300%2C140&ssl=1 300w" sizes="(max-width: 443px) 100vw, 443px" data-recalc-dims="1" /></a>

The user will then be asked how they want to join the meeting.

<a href="http://masteringlync.com/2013/09/28/lync-server-2013-cu2-and-windows-xp-workstations/pic3-7/" rel="attachment wp-att-414"><img class="alignnone wp-image-414 size-medium" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/09/pic3-300x265.png?resize=300%2C265&#038;ssl=1" alt="pic3" width="300" height="265" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/09/pic3.png?resize=300%2C265&ssl=1 300w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/09/pic3.png?w=372&ssl=1 372w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" /></a>

If you choose the &#8220;Join using corporate credentials&#8221; than you will be presented to login with your credentials.  If your computer is domain joined than your computer will login for you.  This will give your Windows XP IE6/IE7 users the full experience.

NOTE: This does not require you to modify the set-cswebservicesconfiguration setting of showing the attendee client.

**Windows XP and IE8**

In a complete twist of fate if you have updated your computer to IE8 you no longer have the Lync 2010 Attendee Client option as the IE8 browser is considered &#8220;full featured&#8221; and will launch into LWA.  Of course you will have the limitations listed above of no Audio/Video.

<span style="text-decoration: underline">Option #1 (Supported)</span>

Live with it.  Seriously, I had to list the only supported option available to you.  You could of course deploy dial-in conferencing as mentioned above but it doesn&#8217;t fix the problem.

<span style="text-decoration: underline">Option #2 (Unsupported)</span><a href="http://masteringlync.com/2013/09/28/lync-server-2013-cu2-and-windows-xp-workstations/pic4-6/" rel="attachment wp-att-415"><img class="alignright wp-image-415 size-full" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/09/pic4.png?resize=346%2C98&#038;ssl=1" alt="pic4" width="346" height="98" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/09/pic4.png?w=346&ssl=1 346w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/09/pic4.png?resize=300%2C85&ssl=1 300w" sizes="(max-width: 346px) 100vw, 346px" data-recalc-dims="1" /></a>

Have the Lync 2013 presenter invite the OCS 2007 R2 user to the meeting.  The Lync presenter simply does an **Invite More People** and invites the OCS 2007 R2 user to the meeting.  In this scenario, the OCS user will be an in-bound toaster pop informing them they are being invited to the meeting.  The user is able to participate in all modalities (Audio, Video and Presentation) via the OCS 2007 R2 client.

<a href="http://masteringlync.com/2013/09/28/lync-server-2013-cu2-and-windows-xp-workstations/pic5-5/" rel="attachment wp-att-416"><img class="alignnone wp-image-416 size-medium" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/09/pic5-300x226.png?resize=300%2C226&#038;ssl=1" alt="pic5" width="300" height="226" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/09/pic5.png?resize=300%2C226&ssl=1 300w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/09/pic5.png?resize=768%2C579&ssl=1 768w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/09/pic5.png?w=1019&ssl=1 1019w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" /></a>

<span style="text-decoration: underline">Option #3 (Unsupported)</span>

In Lync 2010 there was an option to **ShowJoinUsingLegacyClientLink** that could be set via the set-cswebservicesconfiguration.  According to TechNet, this feature has been deprecated in Lync 2013 however it still works.  So if you enable this option you have the ability to join via the OCS 2007 R2 client.  This is similar to the above option but the end user has the ability to join themselves.

<a href="http://masteringlync.com/2013/09/28/lync-server-2013-cu2-and-windows-xp-workstations/pic6-5/" rel="attachment wp-att-419"><img class="alignnone wp-image-419 size-medium" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/09/pic6-300x228.png?resize=300%2C228&#038;ssl=1" alt="pic6" width="300" height="228" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/09/pic6.png?resize=300%2C228&ssl=1 300w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/09/pic6.png?w=555&ssl=1 555w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" /></a>

NOTE: The &#8220;Join using Office Communicator&#8221; link is only available if you use IE8.  If you are using IE6/IE7 than your only option is to use the Lync Attendee Client as described above.

**Conclusion**

Hopefully this gives you a good overview of the options presented to you as an organization with your legacy Windows XP workstation.  Of course the most preferable option would be to upgrade those workstations but sometimes that option simply doesn&#8217;t exist.