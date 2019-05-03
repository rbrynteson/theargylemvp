---
id: 184
title: Patching OWAS 2013 for Lync Server
date: 2013-03-13T16:12:28+00:00
author: Richard Brynteson
layout: post
guid: http://masteringlync.com/?p=184
permalink: /2013/03/13/patching-owas-2013-for-lync-server/
st_layout_box:
  - right
categories:
  - Lync Server 2013
tags:
  - OWAS
---
So we now have our first official patch for OWAS 2013 and you might be asking yourself, how do I patch this server?  Well the patch is available via Windows Update or here (<http://www.microsoft.com/en-us/download/details.aspx?id=36981>).  However, it should be noted that if you use Windows Update or just patch your server with the above link you will most likely find your OWAS deployment broken and not working.  Others have reported such issues as high CPU spikes and crashes (personally I&#8217;ve just had the broken OWAS server as a result).

So when I deployed this patch three days ago I went ahead and removed the farm and recreated it and everything was good.  I have tested this in three different places now and in every case the removal of the farm and recreation of it fixed the problem.

[EDIT &#8211; So this is normal apparently]

So deep in the TechNet belly of the beast you will find this article:

<http://technet.microsoft.com/en-us/library/jj966220.aspx>

(Thanks to Jens Trier @ Microsoft for pointing out this location.)

According to these directions you do indeed need to remove your Office Web Apps farm, apply the patch and then recreate the farm otherwise the above symptoms will exist.

It&#8217;s an easy enough fix to the problem but it&#8217;s just a matter of finding that fix.

&nbsp;