---
id: 231
title: 'Understanding Lync Discover and It&#8217;s Problems'
date: 2013-04-10T23:01:07+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=231
permalink: /2013/04/10/understanding-lync-discover-and-its-implications/
st_layout_box:
  - right
panels_data:
  - 'a:0:{}'
categories:
  - Lync Server 2013
---
So if you have been following the progression of Lync Server 2013 it is well documented that the client now uses lyncdiscover and lyncdiscoverinternal.sipdomain.com as it&#8217;s first registration lookup method.  This is a fundamental shift from Lync Server 2010 which uses SRV records first.  If you haven&#8217;t seen the full details of how lyncdiscover is used you should go here for a great detailed post about it:

<http://blog.schertz.name/2012/12/lync-2013-client-autodiscover/>

Now that we understand the LyncDiscover process there is an important side-affect to this login problem.  Essentially, when you login to the Lync 2013 client and your SipDomain and internal web services domain are different (i.e. sipdomain.com vs domain.local for example) you will end up with this prompt:

[<img title="Errorr" src="https://i0.wp.com/masteringlync.com/files/2013/04/Errorr_thumb.jpg?w=800&#038;ssl=1" alt="Errorr" border="0" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.com/files/2013/04/Errorr.png)

Now the folks [over here](http://www.ntsystems.it/post/Lync-2013-(Client)-and-LyncDiscoverInternal.aspx) have shown the problem and details the process of where in the registry or Office 2013 ADM files you need to add the Trusted Domain information.  I wanted to take a moment to detail why this is happening.

When your client does a lookup for lyncdiscoverinternal.sipdomain.com it is starting the new discovery process.  This process first introduced in Lync 2010 for mobility, allowed the clients to find their home web services.  Now that the client also uses this process for discovery we can have sip domain name and services domain name mismatch.

A little history.  In Lync 2010 when you did automatic login, you would get around this by having your SRV record (\_sipinternaltls.\_tcp.sipdomain.com) always point to sip.sipdomain.com and never to the actual pool name, in particular if your pool name was different than your SIP Domain name.  If you did this (often called Cross–Domain SRV record) the Lync Client would return an error about a trust issue.

When we have the SIP Domain and the Web Services Domain are different we are actually introducing this cross domain issue.  For example, let&#8217;s say my sip domain is @masteringlync.com and my internal domain is litwareinc.com.  When I do my lookup with lyncdiscover.masteringlync.com it will return the internal web services URL of my litwareinc.com pool, in this case int-litwarepool.litwareinc.com.

We can see this in the trace:

[<img title="Pool1" src="https://i2.wp.com/masteringlync.com/files/2013/04/pool1_thumb.jpg?w=800&#038;ssl=1" alt="Pool1" border="0" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.com/files/2013/04/pool1.png)

So at this point in time my client will prompt because it doesn&#8217;t trust the litwareinc.com domain.  Now I can get around this by:

1. Adding litwareinc.com to my trusted domain list via registry or GPO changes.  See the above post for how to do this.

<del>2. Use an internal web services URL name that is the same as the sip domain.  However, this only works/solves my problem if I have only one sip domain in my environment.  If I have several SIP domains than this problem will still exist.  Additionally, solution #2 doesn&#8217;t help with those who do standard edition server deployments as the internal web services URL is the same as your server name and you cannot change this in topology builder.</del>

UPDATE

2. So changing the web services URL doesn&#8217;t have the intended result you would think you want.  So doing more traces of this with Extra Team&#8217;s Tom Pacyk (<http://www.confusedamused.com/>) we found some interesting behavior.  So in our testing what we did was change the internal web services URL to one that is the same as the sipdomain and the problem remains.  So the question became, what exactly was trying to access the pool name that was outside the sipdomain.  So what if we generated a new internal certificate for just the internal IIS instance would that solve the problem.

CN=lws-int.sipdomain.com

In this scenario, the prompt goes away, but why?  Looking at a Fiddler trace of traffic we can see the request for the web ticket fails causing the autodiscover service to fail with a 401 unauthorized.  Once we have failed on the autodiscover services, the client does what it&#8217;s supposed to do and fails back to SRV records and login is successful.  From the client perspective it looks like it&#8217;s successful now and not causing the prompt but really we just broke the Lync Autodiscover Services.

So essentially the moral of the story is that there isn&#8217;t a clear work around.  If you don&#8217;t want to have to deal with the GPO deployment you need to have your SIP Domain, Web Services URL and Pool Name all be the same.  This &#8216;fix&#8217; only works if you have a single domain name and a Enterprise Edition pool.  If you are a standard edition pool there is no easy work around right now.  Thanks again to Tom for first noticing the SRV failover.

NOTE: It has been suggested to change your internal web services URL on a standard edition pool via PowerShell.  Although you can do that, it is NOT SUPPORTED and should never be done in production.  You have no clue what else could be broken if you do this.

Hopefully that makes sense why this is happening and how to resolve it&#8217;s issue.

UPDATED: CU1 has a few more items returned.  The problem still exists of course but want to keep the screenshot accurate.

<a href="http://masteringlync.com/2013/04/10/understanding-lync-discover-and-its-implications/imag2/" rel="attachment wp-att-236"><img class="alignnone wp-image-236 size-full" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/04/imag2.png?resize=649%2C281&#038;ssl=1" alt="imag2" width="649" height="281" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/04/imag2.png?w=649&ssl=1 649w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/04/imag2.png?resize=300%2C130&ssl=1 300w" sizes="(max-width: 649px) 100vw, 649px" data-recalc-dims="1" /></a>

&nbsp;

&nbsp;