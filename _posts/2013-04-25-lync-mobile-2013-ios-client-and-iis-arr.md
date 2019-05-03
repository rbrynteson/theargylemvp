---
id: 273
title: Lync Mobile 2013 iOS Client and IIS ARR
date: 2013-04-25T02:06:18+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=273
permalink: /2013/04/25/lync-mobile-2013-ios-client-and-iis-arr/
st_layout_box:
  - right
panels_data:
  - 'a:0:{}'
categories:
  - Lync Server 2013
---
[<img class="alignright size-medium wp-image-1022" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/04/photo1-169x300.png?resize=169%2C300&#038;ssl=1" alt="photo" width="169" height="300" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/04/photo1.png?resize=169%2C300&ssl=1 169w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/04/photo1.png?w=225&ssl=1 225w" sizes="(max-width: 169px) 100vw, 169px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/04/photo1.png)So a [previous post](http://masteringlync.com/2013/02/12/using-iis-application-request-routing-arr-as-a-tmg-replacement/) of mine I detailed the installation and use of IIS ARR with Windows 2012 as an alternative to using Forefront TMG as your reverse proxy solution. Microsoft posted a [NextHop](http://blogs.technet.com/b/nexthop/archive/2013/02/19/using-iis-arr-as-a-reverse-proxy-for-lync-server-2013.aspx) article a few days later detailing the same information.

So if you have been using IIS ARR you most likely have found that everything was working great however you may have run into a small issue with iOS devices (see the screenshot to the right). The issue is due to how iOS does notifications and we are getting a timeout from the reverse proxy.

A little background:

In the previous mobile client for iOS and Windows Phone they leveraged the Office 365 for push notifications. In the Lync 2013 iOS mobile client they use a native iOS feature called VoIP Socket. What is happening under the hood is the Lync Client is sending a &#8220;hanging get&#8221; to the Lync Server and specifically to the UCWA application that was introduced in CU1. This hanging get last for at least 10 minutes and is refreshed so the server knows the client is still alive &#8211; essentially a presence registration is occurring in Lync. So what is happening is that the IIS timeout is causing issues with the hanging get. So if you are seeing the problem to the side, I would suggest changing your timeout from 180 or 200 seconds to 960 seconds (16 minutes). This will ensure your client doesn&#8217;t timeout on the &#8220;hanging get&#8221; that is associated with push notification.

Once I review the logs I&#8217;ll post the relevant parts of the logs showing where the timeout is happening.

Lastly, for completeness I should note that only the Windows Phone Mobile clients still use the Office 365 for push notifications. The new Android client runs in the background and does the notifications differently.

UPDATE: Here is the UCWA application creating the timeout values:

_CEventChannelManager.cpp/270:Creating UCWA event channel request with Remote timeout=900, Local timeout=920_