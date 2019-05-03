---
id: 624
title: 'Quick Tip: iOS Client won&#8217;t login'
date: 2014-02-04T23:21:44+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=624
permalink: /2014/02/04/quick-tip-ios-client-wont-login/
st_layout_box:
  - right
panels_data:
  - 'a:0:{}'
categories:
  - Lync Server 2013
  - Quick Tips
---
This is the first of my quick tip items &#8211; less in-depth analysis and more &#8220;how to fix&#8221;.

**Scenario**: Lync iOS user running iOS 7.x, connected via wireless connection.

**Issue**: User was unable to login to client.  They were met immediately with a &#8220;unable to connect to server&#8221; error message.  We found that if the user were to move to another wireless connection they were able to login without any issue.

**Log File**: Upon review of the log file I found several messages like the following:

_Failed to get http proxy setting using <http://wpad/wpad.dat> for UcwaAutoDiscoveryRequest for <user@domain.com>_

This would happen about 10 times through out the single login experience.

**Solution**: The device was attempting to discover corporate proxy settings due to settings on the client.  Within the Settings | Wi-Fi on the iOS device.

[<img class="alignnone wp-image-625 size-medium" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/02/photo-169x300.png?resize=169%2C300&#038;ssl=1" alt="photo" width="169" height="300" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/02/photo.png?resize=169%2C300&ssl=1 169w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/02/photo.png?resize=577%2C1024&ssl=1 577w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/02/photo.png?w=640&ssl=1 640w" sizes="(max-width: 169px) 100vw, 169px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.com/files/2014/02/photo.png)

Change HTTP Proxy from Auto to Off.  The WPAD errors immediately went away and the device was able to connect without issue.