---
id: 25
title: Using Fiddler to troubleshoot address book download issues
date: 2011-08-13T21:18:31+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=25
permalink: /2011/08/13/using-fiddler-to-troubleshoot-address-book-download-issues/
st_layout_box:
  - right
categories:
  - Lync Server 2010
---
When troubleshooting external address book download issues, sometimes you simply don&#8217;t get enough information.  The Lync client itself simply says unable to download address book.  Before we look at the problem, let&#8217;s look at a common setup of Lync.

An external request to the address book is served from the Lync client to the external web address.  That address might be something like lws.domain.com from the outside.  This address would be sent to a reverse proxy with a public certificate.  That reverse proxy is typically a TMG or ISA server.  Because the traffic is https, it is difficult to get additional information about the connection.  So this is where Fiddler comes into play.  Fiddler is a Web Debugging tool that allows you to get detailed http/s information.  So for our test, you need to download and install [Fidder2](http://www.fiddler2.com/fiddler2/) and then launch the Lync client.  When the Lync client attempts to connect to the Reverse Proxy, all we see is this:

[<img src="https://i1.wp.com/blogs.inetium.com/resized-image.ashx/__size/550x0/__key/CommunityServer.Blogs.Components.WeblogFiles/uc/Screen-Shot-2011_2D00_08_2D00_13-at-11.18.54-AM.png?w=800&#038;ssl=1" alt="" border="0" data-recalc-dims="1" />](https://i0.wp.com/blogs.inetium.com/cfs-file.ashx/__key/CommunityServer.Blogs.Components.WeblogFiles/uc/Screen-Shot-2011_2D00_08_2D00_13-at-11.18.54-AM.png)

This is normal.  All we see is a secure HTTP connection.  So no additional details.  However, Fiddler includes a great feature to decrypt HTTPS traffic.  To do this, go to Tools | Fiddler Options and choose the HTTPS tab.  Click Decrypt HTTPS traffic and ignore encryption errors:

[<img src="https://i2.wp.com/blogs.inetium.com/resized-image.ashx/__size/550x0/__key/CommunityServer.Components.UserFiles/00.00.00.31.23/Screen-Shot-2011_2D00_08_2D00_10-at-11.00.54-AM.png?w=800&#038;ssl=1" alt="" border="0" data-recalc-dims="1" />](https://i1.wp.com/blogs.inetium.com/cfs-file.ashx/__key/CommunityServer.Components.UserFiles/00.00.00.31.23/Screen-Shot-2011_2D00_08_2D00_10-at-11.00.54-AM.png)

You will be prompted with an &#8220;are you sure&#8221; type of message.  You need to say Yes to this.  Basically, what fiddler does is proxy all traffic so by saying yes you will decrypt all traffic.  So now we relaunch Fiddler (must restart for change to take effect) and then relaunch the Lync client.  Now we see much more information:

[<img src="https://i1.wp.com/blogs.inetium.com/resized-image.ashx/__size/550x0/__key/CommunityServer.Components.UserFiles/00.00.00.31.23/Screen-Shot-2011_2D00_08_2D00_10-at-10.59.59-AM.png?w=800&#038;ssl=1" alt="" border="0" data-recalc-dims="1" />](https://i0.wp.com/blogs.inetium.com/cfs-file.ashx/__key/CommunityServer.Components.UserFiles/00.00.00.31.23/Screen-Shot-2011_2D00_08_2D00_10-at-10.59.59-AM.png)

Now we can see the actual HTTPS connect and exactly what it&#8217;s trying to connect to.  Additionally, we can click on the traffic and see additional details to the error.

[<img src="https://i2.wp.com/blogs.inetium.com/resized-image.ashx/__size/550x0/__key/CommunityServer.Components.UserFiles/00.00.00.31.23/Screen-Shot-2011_2D00_08_2D00_10-at-11.00.23-AM.png?w=800&#038;ssl=1" alt="" border="0" data-recalc-dims="1" />](https://i0.wp.com/blogs.inetium.com/cfs-file.ashx/__key/CommunityServer.Components.UserFiles/00.00.00.31.23/Screen-Shot-2011_2D00_08_2D00_10-at-11.00.23-AM.png)

In our case, you can see we are getting an Invalid Web Ticket error message.  With this informaiton, we know we are attempting to download the correct information and we aren&#8217;t getting an authentication error, but rather something else.  Authentication erorrs are typically on the IIS side.  Address book errors with a web ticket error is typically happening on the reverse proxy.  In the end, in this case our problem was with the TMG server, as &#8220;Pass orginial host header&#8221; was not checked on the listener, causing a problem IIS rewrite process.  Checking that fixed our problem and the address book was able to download successfully.

Hopefully someone finds this information helpful.