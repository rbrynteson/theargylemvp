---
id: 1251
title: 'Where is my ICE ICE Baby?  V6 is gone!'
date: 2016-03-01T15:53:45+00:00
author: Richard Brynteson
layout: post
guid: http://masteringlync.com/?p=1251
permalink: /2016/03/01/where-is-my-ice-ice-baby-v6-is-gone/
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
  - Skype for Business
---
Before we get into the changes in the Office 2016 and Skype for Business 16.x client a quick note of what ICE is.  According to <a href="http://download.microsoft.com/download/1/6/F/16F4E321-AA6B-4FA3-8AD3-E94C895A3C97/%5BMS-SDPEXT%5D.pdf" target="_blank">Microsoft&#8217;s SDP Extension </a>document:

_**Interactive Connectivity Establishment (ICE)**: A methodology that was established by the Internet Engineering Task Force (IETF) to facilitate the traversal of network address translation (NAT) by media._

What does that really mean?  It&#8217;s the method by which the clients share information to stand up media, in particular, through a NAT or firewall.  It&#8217;s the basis for all audio/video traffic in SfB (and all the way back to OCS 2007).

If you want a great article on how SDP and ICE work, you should check out this article from the NextHop Team: <a href="https://blogs.technet.microsoft.com/nexthop/2009/04/22/how-communicator-uses-sdp-and-ice-to-establish-a-media-channel/" target="_blank">How Communicator Uses SDP and ICE To Establish a Media Channel</a>.

**What Has Changed?**

Starting in OCS 2007 R2, Microsoft started offering both ICE v6 and ICE v19 in the initial SDP.  ICE v6 was considered the fallback protocol.  If you looked at the SDP of your initial INVITE you would see something like this:

<a href="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/03/1.png" rel="attachment wp-att-1252"><img class="alignnone size-full wp-image-1252" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/03/1.png?resize=666%2C283&#038;ssl=1" alt="1" width="666" height="283" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/03/1.png?w=666&ssl=1 666w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/03/1.png?resize=300%2C127&ssl=1 300w" sizes="(max-width: 666px) 100vw, 666px" data-recalc-dims="1" /></a>

You can see the Content-Disposition includes a ms-proxy-2007fallback tag and the initial ICE candidate list is formatted in v6.  The easiest way to know is by looking right after the a=candidate we see they are not numbered.

If you look at a SfB/Office 2016 initial invite it will look like this:

<a href="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/03/2.png" rel="attachment wp-att-1253"><img class="alignnone size-full wp-image-1253" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/03/2.png?resize=500%2C314&#038;ssl=1" alt="2" width="500" height="314" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/03/2.png?w=500&ssl=1 500w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/03/2.png?resize=300%2C188&ssl=1 300w" sizes="(max-width: 500px) 100vw, 500px" data-recalc-dims="1" /></a>

When looking at this invite, we see that there is no reference to the fallback option and the SDP itself starts with the v19 formatted candidate list.  The easy way to know is the a=candidate is formatted as 1 1 and 1 2 (for the first pair) and you see IPv6 support, which wasn&#8217;t included in v6.

I&#8217;ve tested this across multiple workstations running Office 2016/SfB 16.x and they all show the same thing.  I have reached out to Microsoft to confirm this was intended and not a bug but haven&#8217;t heard anything back yet.

**Why Do We Care?**

First, I don&#8217;t recall Microsoft ever telling anyone they were dropping ICE v6 support from the client.  Obviously, it would be nice if they told the community of semi-major plumbing changes between clients.

Second, there are some interrop issues you might need to worry about.  Obviously, the OCS 2007 client didn&#8217;t support ICE v19, so if you were calling a customer who is still on OCS 2007 your call will not connect.

Third, Exchange 2007 didn&#8217;t support ICE v19, so if your customer is still using an old 2007 Exchange UM Server you won&#8217;t be able to leave a voicemail for that user.  This is how I initial learned of the problem.  We were troubleshooting an issue where we could not leave a VM for a customer after they patched their Lync 2010/Exchange 2007 environment.

&nbsp;

In the end, this is primarily random trivia for you to remember, but if you see a problem connecting to some really old clients/exchange servers you might know why now.

&nbsp;