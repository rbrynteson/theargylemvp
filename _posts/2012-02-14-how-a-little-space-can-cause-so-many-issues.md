---
id: 14
title: How a little space can cause so many issues.
date: 2012-02-14T21:06:21+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=14
permalink: /2012/02/14/how-a-little-space-can-cause-so-many-issues/
st_layout_box:
  - right
categories:
  - Lync Server 2010
---
When a user attempts to login to they use DNS and SRV records to lookup their home server.  They start by making three parallel requests for:

\_sipinternaltls.\_tcp.domain.com

\_sipinternal.\_tcp.domain.com

\_sip.\_tls.domain.com

The first two are defined in your internal DNS with the last one being published externally.  The clients use these SRV records to lookup the service (published via A record) of where to automatically login.  I was working on a client recently, where no matter what I did I could not get automatic login to work externally.  To complicate matters, I did not have access to the DNS tools (as it was hosted) so I had to work with was what I saw on the client.  When I would do an NSLookup, I would see exactly what I expected to see.  The A record being returned and I could resolve the A record, however, the Lync client would continue to throw an error that it could not resolve the server name.

So I did traces on the server, just to verify, and indeed the client would not even attempt to connect to the edge server.  If I manually configured the client, it would work exactly as expected.  This is where the local log files and snooper come into play.  If you have not yet installed Snooper on your local machine, you should.  Just like the server log files, your client SIP Stack logs can be parsed by Snooper and provide you with valuable information.

So steps one was to enable client side logging.  This can be done either at the client (Gear Box | Options | General) or via the set-csclientpolicy powershell for the company.  Here you can see my settings enabled via policy.

[<img src="https://i0.wp.com/blog.avtex.com/wp-content/uploads/2012/02/Screen-Shot-2012-02-14-at-3.57.38-PM-300x61.png?resize=300%2C61&#038;ssl=1" alt="" width="300" height="61" data-recalc-dims="1" />](https://i2.wp.com/blog.avtex.com/wp-content/uploads/2012/02/Screen-Shot-2012-02-14-at-3.57.38-PM.png)

Second item was to reproduce the issue.  Once done, you will have a log file named Communicator-uccapi-0.uccapilog.  This can be found at:

c:users<username>tracing

[<img src="https://i2.wp.com/blog.avtex.com/wp-content/uploads/2012/02/image11-300x76.png?resize=300%2C76&#038;ssl=1" alt="" width="300" height="76" data-recalc-dims="1" />](https://i0.wp.com/blog.avtex.com/wp-content/uploads/2012/02/image11.png)

So the client does exactly as expected.  It fails to find the internal names and moves onto the external names, which it finds correctly.  But notice the resolution _(sip.domain.com )_.  Anything look strange about that reply?  A space after the .com?  It couldn’t be, but it was.  When the SRV record was created they accidentally added a space at the name.  Didn’t think it was possible but after they re-entered the name removing the space, login problems were solved.

Goes to show you how important understanding the login process is and knowing how to read the logs.

Enjoy.