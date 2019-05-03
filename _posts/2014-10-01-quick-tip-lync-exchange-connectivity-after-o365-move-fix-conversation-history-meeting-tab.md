---
id: 845
title: 'Quick Tip: Lync + Exchange Connectivity after O365 Move (Fix Conversation History + Meeting Tab)'
date: 2014-10-01T15:34:00+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=845
permalink: /2014/10/01/quick-tip-lync-exchange-connectivity-after-o365-move-fix-conversation-history-meeting-tab/
st_layout_box:
  - right
panels_data:
  - 'a:0:{}'
categories:
  - Lync Server 2013
---
In this quick tip we look at the Lync client behavior after a users mailbox has been moved from on-premise exchange to Office 365.

**Problem**

After a migration from your on-prem mailbox to Office 365 you may find your Lync Client is unable to connect to the Exchange Server and pull Conversation History and Meeting Tab.  You would find the following error.

[<img class="alignnone wp-image-846 size-full" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/10/pic1.png?resize=369%2C94&#038;ssl=1" alt="pic1" width="369" height="94" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/10/pic1.png?w=369&ssl=1 369w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/10/pic1.png?resize=300%2C76&ssl=1 300w" sizes="(max-width: 369px) 100vw, 369px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.com/files/2014/10/pic1.png)

**Where Does the Client Store This Info?**

As you can most likely guess, the Lync Client stores all of this information within the Registry.  If we browse to HKCUSoftwareMicrosoftOffice15.0Lyncuser@domain.comAutodiscovery

[<img class="alignnone wp-image-847 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/10/pic2.png?resize=612%2C507&#038;ssl=1" alt="pic2" width="612" height="507" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/10/pic2.png?w=612&ssl=1 612w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/10/pic2.png?resize=300%2C249&ssl=1 300w" sizes="(max-width: 612px) 100vw, 612px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.com/files/2014/10/pic2.png)

Here, we can see the available EWS and various other URLs are all pointing outlook.office365.com.  What I&#8217;ve seen on various Lync Clients there are situations where this information doesn&#8217;t update and the client is still pointing to the on-premise Exchange deployment.  For whatever reason, the client may never find it&#8217;s way home and the on-premise Exchange doesn&#8217;t forward the requests correctly.

The fix is easy.  Close the Lync client, and delete HKCUSoftwareMicrosoftOffice15.0Lyncuser@domain.comAutodiscovery key.  Once you start the Lync Client again, your client will go through the Exchange Autodiscover process and get the correct location will be published into the registry.