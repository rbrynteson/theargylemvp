---
id: 1296
title: 'Quick Tip: Polycom VVX, Pin Auth, Login Names'
date: 2016-10-13T04:08:40+00:00
author: Richard Brynteson
layout: post
guid: http://masteringlync.com/?p=1296
permalink: /2016/10/13/quick-tip-polycom-vvx-phone-cannot-login/
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
Another day and another odd ball issue with Skype for Buiness and Polycom VVX Phones.  So hopefully this helps someone along the way.

**Scenario**

So I have an existing common area phone that is attempting to pin auth into a server.  The customer has CX600 phones and looking to replace those phones with new VVX phones as part of a larger replacement.  When the customer attempted to pin auth with their existing accounts, the phones will not pin auth correctly.  We pulled out a CX 600 phone again and was able to login without any issues.

One of the big advantages of a VVX phones is the ability to look at the logs.  When we opened up the Diagnostics and Logs we see this:

<a href="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/10/1.jpg" rel="attachment wp-att-1299"><img class="alignnone wp-image-1299 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/10/1.jpg?resize=800%2C323&#038;ssl=1" alt="1" width="800" height="323" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/10/1.jpg?w=1059&ssl=1 1059w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/10/1.jpg?resize=300%2C121&ssl=1 300w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/10/1.jpg?resize=768%2C310&ssl=1 768w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/10/1.jpg?resize=1024%2C414&ssl=1 1024w" sizes="(max-width: 800px) 100vw, 800px" data-recalc-dims="1" /></a>

If you notice in the logs, it&#8217;s trying to register to &#8216;domain.c&#8217;.  As you can most likely guess, this isn&#8217;t the domain name for the organization.

**Solution**

So we have a log of common area phones so we are using a 36 digit guid as the SIP address.  The domain itself is 24 characters long.  For example:

sip:ca7903b3-59ef-4dd0-af4c-ea8cee814faa@abababababababababab.com

Well, as you guessed, there is apparently a maximum number of characters in the SIP name for the VVX phones in order to Pin Auth.  We shortened the guid by 2 letters and suddenly the phone was able to login without any issues.  We tested all the way to 5.4.5 software release.

Hopefully this is an easy fix on Polycom&#8217;s part.  I tried to create a ticket for this but I&#8217;m not allowed to so they closed it immediately.   🙁

&nbsp;