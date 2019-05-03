---
id: 1232
title: 'Quick Tip: Polycom VVX File Names'
date: 2016-01-29T15:07:43+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=1232
permalink: /2016/01/29/quick-tip-polycom-vvx-file-names/
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
tags:
  - Polycom VVX
---
As the world might know, I&#8217;ve been playing around with VVX provisioning files for the better part of a few months.  We added some basic Polycom Provisioning Server support to the skypevalidator.com website two months ago.  I&#8217;ve been working on taking this a step further with individual files and found an oddity.

**Issue:**

My provisioning server has both a default file 000000000000.cfg and a [MAC-ADDRESS].cfg file.

[<img class="alignnone size-full wp-image-1233" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/01/1.png?resize=588%2C44&#038;ssl=1" alt="1" width="588" height="44" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/01/1.png?w=588&ssl=1 588w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/01/1.png?resize=300%2C22&ssl=1 300w" sizes="(max-width: 588px) 100vw, 588px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/01/1.png)

Regardless how how many times I rebooted the phone, set it back to defaults, the device would always use the configuration from the 000000000000.cfg file instead.  If you notice, my file name for the MAC Address phone file has a capital F and E.  Why, because I was lazy and copy/pasted from the VVX Web GUI.

This was tested on a Linux Server as the provisioning server with HTTPS.

**Solution:**

After spending way too many hours on this I decided to try this on a Windows FTP server and suddenly the file works without any issues.  Thought about it for a little bit and remembered, Linux file system is case sensitive!!!!!

[<img class="alignnone size-full wp-image-1234" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/01/2.png?resize=588%2C47&#038;ssl=1" alt="2" width="588" height="47" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/01/2.png?w=588&ssl=1 588w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/01/2.png?resize=300%2C24&ssl=1 300w" sizes="(max-width: 588px) 100vw, 588px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/01/2.png)

So I went back to my Linux/HTTPS file location and renamed the file to use lower case letters.  A reboot later and it all worked.  Clearly the VVX phones, when they do their MAC Address file request, send it all lowercase and never attempt an uppercase option (and why would they).