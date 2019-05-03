---
id: 680
title: 'Lync March 2014 Update (CU5) &amp; Inline Images (now it&#8217;s broke!)'
date: 2014-03-18T02:03:05+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=680
permalink: /2014/03/18/lync-march-2014-update-cu5-inline-images-now-its-broke/
st_layout_box:
  - right
panels_data:
  - 'a:0:{}'
categories:
  - Lync Server 2013
---
It appears as thought either the previous implementation of inline image copy/paste was working because of a bug or the update introduced a new bug.  So what is the issue?  (SEE BOTTOM FOR UPDATE)

Updating my workstation with the March 2014 patch (<http://support.microsoft.com/kb/2863908>) my client shows this version now:

[<img class="alignnone wp-image-681 size-full" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/03/pic1.png?resize=411%2C93&#038;ssl=1" alt="pic1" width="411" height="93" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/03/pic1.png?w=411&ssl=1 411w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/03/pic1.png?resize=300%2C68&ssl=1 300w" sizes="(max-width: 411px) 100vw, 411px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.com/files/2014/03/pic1.png)

When a user attempts to copy/paste a picture inline to the IM window you get this message:

[<img class="alignnone wp-image-682 size-full" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/03/pic21.png?resize=266%2C108&#038;ssl=1" alt="pic2" width="266" height="108" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.com/files/2014/03/pic21.png)

Prior to this patch, the copy/paste of images to the inline conversation window worked fine.  In the organization we currently had peer-to-peer file transfer off for the customer network due to security concerns.  Since the inline image is simply utilizing that feature, we decided to enabled peer-to-peer file transfer in the conferencing policy.

[<img class="alignnone wp-image-683 size-full" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/03/pic31.png?resize=451%2C177&#038;ssl=1" alt="pic3" width="451" height="177" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/03/pic31.png?w=451&ssl=1 451w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/03/pic31.png?resize=300%2C118&ssl=1 300w" sizes="(max-width: 451px) 100vw, 451px" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.com/files/2014/03/pic31.png)

Once I enabled the policy and logged out and back into my client, I was able to inline image again.

[<img class="alignnone wp-image-684 size-full" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/03/pic41.png?resize=429%2C344&#038;ssl=1" alt="pic4" width="429" height="344" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/03/pic41.png?w=429&ssl=1 429w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/03/pic41.png?resize=300%2C241&ssl=1 300w" sizes="(max-width: 429px) 100vw, 429px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.com/files/2014/03/pic41.png)

So there you have it.  If you update to the March 2013 and need to use inline images, make sure to enable peer-to-peer file transfers in your conference policy.

UPDATE: I have confirmed this was an intended change with part of SP1 due to some concerns around archiving and the fact that images were not stored in the archiving database.

UPDATE: To be clear, the client updates DOESN&#8217;T change your server policy.  The issue is, if you had the peer-to-peer file transfer disabled previously this feature worked fine.  Once you apply this patch, if you don&#8217;t enable peer-to-peer file transfer it no longer works.