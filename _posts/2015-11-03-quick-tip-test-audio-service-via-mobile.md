---
id: 1202
title: 'Quick Tip: Test Audio Service via Mobile'
date: 2015-11-03T22:39:50+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=1202
permalink: /2015/11/03/quick-tip-test-audio-service-via-mobile/
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
  - Lync Server 2010
  - Lync Server 2013
  - Skype for Business
---
So we were presented with an interesting question.  Can we use the Audio Test Service on the mobile client.  When I tried to search for the object on my Windows Phone it didn&#8217;t find anything (I&#8217;ll be honest, could be searchable from other mobile platforms but I didn&#8217;t test it) but we came up with a pretty simple solution to ensure we could do this.

Step 1: From your fat client, make a call to the Audio Test Service and add the user to your contact list.

[<img class="alignnone wp-image-1203 size-medium" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/11/1-300x139.png?resize=300%2C139&#038;ssl=1" alt="1" width="300" height="139" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/11/1.png?resize=300%2C139&ssl=1 300w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/11/1.png?resize=768%2C356&ssl=1 768w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/11/1.png?w=872&ssl=1 872w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/11/1.png)

Step #2 &#8211; Go to your Windows Mobile Client and there it is.

[<img class="alignnone wp-image-1204 size-medium" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/11/client-169x300.png?resize=169%2C300&#038;ssl=1" alt="client" width="169" height="300" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/11/client.png?resize=169%2C300&ssl=1 169w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/11/client.png?resize=576%2C1024&ssl=1 576w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/11/client.png?w=720&ssl=1 720w" sizes="(max-width: 169px) 100vw, 169px" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/11/client.png)

Simple solution but handy to be able to make/call the Audio Test Service before making SfB Calls over the carrier network.