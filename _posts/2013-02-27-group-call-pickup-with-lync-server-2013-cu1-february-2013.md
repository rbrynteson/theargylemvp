---
id: 179
title: Group Call Pickup with Lync Server 2013 CU1 (February 2013)
date: 2013-02-27T21:23:13+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=179
permalink: /2013/02/27/group-call-pickup-with-lync-server-2013-cu1-february-2013/
st_layout_box:
  - right
categories:
  - Lync Server 2013
tags:
  - Group Call Pickup
  - Lync Server 2013 CU1
---
With the addition of Lync Server 2013 CU1 you now have two major new features in terms of Enterprise Voice.  First is Location Based Routing which I have already detailed.  Now we get Group Call Pickup.  Don&#8217;t confuse this with Line Appearance, because they are not the same thing, at all.  What Group Call Pickup does is allows you to grab another persons line if you happen to hear it ringing.  This is of course very different than Delegate Ring or something like that as it requires the end user to configure, instead this is an administrative tool.

Under the covers it leverages the Call Park service and the Call Orbits to allow you to configure this.

Matt Landis does a great job of explaining it here: <http://windowspbx.blogspot.com/2013/02/call-pickup-groups-coming-to-lync.html>

So how do you configure this service:

First, you will need a server that is running the SEFAUtil resource kit tool.  This can be a trusted app server or if you like living on the edge, you can configure an existing front-end as a trusted app server &#8211; but that isn&#8217;t supported and should be used in labs only.

Second, create a Call Park Orbit Range.  This can be created in the GUI but you will need to edit it via PowerShell anyways, so just create it in PowerShell:

New-CsCallParkOrbit -Identity &#8220;Minneapolis&#8221; -NumberRangeStart &#8220;#100&#8221; -NumberRangeEnd &#8220;#199&#8243; -CallParkService pool.domain.com –Type GroupPickup

Third, using the SEFAUtil tool you will enable a set of user(s) to a particular range:

SEFAUtil.exe /server:pool.domain.com user@domain.com /enablegrouppickup:&#8221;#100&#8221;

If successful, you will see SEFAUtil spit out a bunch to the screen and you know it has worked.  If it doesn&#8217;t give you the details of the user, then the tool didn&#8217;t do anything.  Most likely because of a typo or your server isn&#8217;t configured as a trusted application server.

Hope that helps.