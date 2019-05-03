---
id: 817
title: 'Quick Tip: IIS/ARR and Authenticated Traffic Failing'
date: 2014-08-26T19:41:33+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=817
permalink: /2014/08/26/quick-tip-iisarr-and-authenticated-traffic-failing/
st_layout_box:
  - right
panels_data:
  - 'a:0:{}'
categories:
  - Lync Server 2010
  - Lync Server 2013
---
In yet another example of how picky IIS/ARR can be here is a perfect example that took a bit of figuring out.  As a baseline, there was absolutely nothing special about the configuration of IIS/ARR (other than I didn&#8217;t hit the install button myself) but everything else is laid out as I&#8217;ve done before in my [IIS/ARR Guide](http://masteringlync.com/2013/02/12/using-iis-application-request-routing-arr-as-a-tmg-replacement/).

  * Windows 2012 R2
  * IIS ARR 3.0
  * Fully patched server

Externally, meet, dialin and discovery services all seemed to work fine.  The problem came with authenticated traffic and more specifically when the Lync Client would request a web ticket, the process would fail.  So a quick trip into fiddler showed this error:

[<img class="alignnone wp-image-818 size-full" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/08/pic11.png?resize=588%2C34&#038;ssl=1" alt="pic1" width="588" height="34" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/08/pic11.png?w=588&ssl=1 588w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/08/pic11.png?resize=300%2C17&ssl=1 300w" sizes="(max-width: 588px) 100vw, 588px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.com/files/2014/08/pic11.png)

And when I clicked on the request to get more details it was as generic as you could get:

_401 &#8211; Unauthorized: Access is denied due to invalid credentials._

So I decided to verify internal services were working and there were no issues present there.  And I even went to my IIS/ARR server and was able to reach the website directly through IE.  So I knew a problem had to be happening within the IIS/ARR instance itself.  So I decided to build a new server side-by-side and see if I could reproduce the issue.  The only thing different from my guide to here was the fact I didn&#8217;t do the install.  After my install, I swung over the DNS records via host file and everything worked great.  So it was time to find the difference between the two.

After digging from screen to screen I found this:

[<img class="alignnone wp-image-819 size-medium" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/08/pic21-300x97.png?resize=300%2C97&#038;ssl=1" alt="pic2" width="300" height="97" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/08/pic21.png?resize=300%2C97&ssl=1 300w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/08/pic21.png?w=734&ssl=1 734w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.com/files/2014/08/pic21.png)

And there was my problem!  Somehow Windows Authentication (and basic auth but less of an issue) had found it&#8217;s way onto my IIS/ARR Server.  This was not on the server I manually built.  I can only assume that the Web Platform Installer is putting all of these authentication methods on the server by default.

So lesson learned.  Make sure that Windows Authentication is disabled (and feel free to disable basic auth as well) on your IIS/ARR box otherwise instead of passing your credentials through it&#8217;s going to try to authenticate on it&#8217;s own.

&nbsp;

&nbsp;