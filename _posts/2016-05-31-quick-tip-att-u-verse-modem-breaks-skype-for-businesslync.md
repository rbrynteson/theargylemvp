---
id: 1262
title: 'Quick Tip: AT&amp;T U-verse modem breaks Skype for Business/Lync'
date: 2016-05-31T13:52:31+00:00
author: Richard Brynteson
layout: post
guid: http://masteringlync.com/?p=1262
permalink: /2016/05/31/quick-tip-att-u-verse-modem-breaks-skype-for-businesslync/
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
  - Lync Server 2013
  - Skype for Business
---
**Problem**: Audio Distortion Problems

This one comes via Rob (one of our support team members at Time2Market) who ran across this gem of a problem.  We had a support ticket that calls to a single person was failing and having serious audio distortion problems.  Lots of different tests were done including packet captures, speed tests and much more.  Everything looked great on the wire except just randomly the calls would go bad really fast and remain that way.

**Solution**: It&#8217;s always the network dummy

After narrowing down that the user had recently received an update to their modem we had a place to look.

<a href="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/05/1.jpg" rel="attachment wp-att-1263"><img class="alignnone size-full wp-image-1263" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/05/1.jpg?resize=697%2C615&#038;ssl=1" alt="1" width="697" height="615" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/05/1.jpg?w=697&ssl=1 697w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/05/1.jpg?resize=300%2C265&ssl=1 300w" sizes="(max-width: 697px) 100vw, 697px" data-recalc-dims="1" /></a>

Here you will notice two settings, both were enabled by default:

  * Flood limit was enabled with a 4 packets per second / 8 max
  * SIP ALG (Application Layer Gateway)

Once both of these items were disabled then everything started working great again.  So moral of the story is always check and see what crazy update AT&T did to your network