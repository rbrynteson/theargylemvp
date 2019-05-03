---
id: 51
title: 'Welcome to the family &#8211; Lync Server 2013'
date: 2012-07-17T01:54:29+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=51
permalink: /2012/07/17/welcome-to-the-family-lync-server-2013/
st_layout_box:
  - right
categories:
  - Lync Server 2013
---
With all of the excitement today about the announcement of Office 2013 we also got the less splashy release of Lync Server 2013 and the inclusion of the Lync 2013 client in Office.  So over the next two weeks we will deep dive through the Lync Server 2013 installation and anything else we happen to find useful in the documentation.  So lets get a few details out of the way now:

[TechNet Documentation is Available Here](http://technet.microsoft.com/en-us/library/gg398616%28v=ocs.15%29) &#8211; [Download Lync Server 2013 Media Here](http://technet.microsoft.com/en-US/evalcenter/hh973393.aspx?wt.mc_id=TEC_118_1_33) &#8211; [Lync 2013 Web Site](http://lync.microsoft.com/en-us/Pages/Lync-2013-Preview.aspx)

Before we jump into any of this, let me officially state that I do not like the Lync 2013 name at all.  I always refer to the versions based on code names, this is Lync 15 but I&#8217;ll deal with it.

**Client Changes**

So let&#8217;s go through a few of the major changes to the client experience with Office/Lync 2013.

System Requirements &#8211; If you haven&#8217;t noticed the system requirements for Office 2013, the following OS are supported: Windows 7, Windows 8, Server 2008 R2, Server 2012.  This means, if you haven&#8217;t gotten off of Windows XP yet, it is time to leave that decade old OS behind.  This also means that the Lync installer is now part of Office 2013 installer.  Which means all of the installation tools you have used in the past for Office you have at your disposal for Lync now.

Virtual Desktop Support &#8211; Lync 2013 supports your virtual desktop deployment.  In the past, you may have done USB redirection from the virtual guest to a CX600, which wasn&#8217;t supported but kinda worked.  Well, now it&#8217;s officially supported and be ready to use it all.  All modalities are supported.

Video Changes &#8211; No more is the old codec of the past and Microsoft is ushering in a new video codec.  As such, we now have support for HD Video in conferences and additionally, we have multiple streams in a conference. So it&#8217;s not the Brady Bunch effect that Polycom offers, but it gives you the most popular people speaking in a conference and if you have one of those typical conferences where there are 5 people or less, you will see them all.

Persistent Chat &#8211; Group Chat is dead and now we have persistent chat instead.  The major change here is that we the client is now built into the Lync client experience.  No more separate client like before.  We will get to the server details in a moment, but it&#8217;s good to know the experience is integrated and it&#8217;s much better.

Tabbed Conversation &#8211; It&#8217;s built into the client and they run down the left side of the client.  Lync 14 had a cute plug-in that you could install but it barely worked and wasn&#8217;t as feature rich as what we get in Lync 2013.

Read all about client changes here &#8211; http://technet.microsoft.com/en-us/library/jj204933%28v=ocs.15%29

**Server Changes**

Again, all of this great info is on TechNet (http://technet.microsoft.com/en-us/library/gg425795%28v=ocs.15%29) but here is a run down of some fantastic options.

**DR/HA** &#8211; So in Lync 14 (there I did it again) we had a fail-over scenario in which resulted in limited functionality mode.  Now, this appears to remain in Lync 2013 but what we have is full replication of user registrar information.  So when you add a contact to your Lync client, it will not only be saved on your primary pool but also to your backup registrar.  Then using the invoke-poolfailover the users will failover to their backup registrar with all of the information, including contacts, conference data and more.  This a great change.  Failback, you guessed it, invoke-poolfailback.

What isn&#8217;t replicated.  Response Groups for now.  I suspect we will get that sooner or later.  What I haven&#8217;t verified yet is if RGS supports early media yet, will test that out here soon and determine.

Another major changes are the fact that Lync 2013 support SQL Mirroring for the back-end database.  This is a welcome change from trying to get SQL Clustering working in some customer environments.

**Role Changes** &#8211; No more AV role.  No more Archiving/Monitoring roles.  All are on the front-ends now.  Director is very much optional now and there is going to be a fairly limited role in which they will be deployed.  XMPP is gone and built into the edge and front-end servers now.  New to the deployment is the Web App Server (known as WAC) which is basically a sharepoint installer for HTML rendering.

**Exchange 2013 Integration** &#8211; Archives can be directed to Exchange now.  And we have high-res photos going to the Exchange database.

**Enterprise Voice Changes** &#8211; One major change that I&#8217;m excited about is that Lync 2013 can modify calling number under the trunk configuration setup.  Another welcome change is the fact that Lync support M-N trunk routing &#8211; sometimes called inter-trunks.  What is really neat about this is the fact that Lync 2013 can act as an interconnect between multiple PBX&#8217;s and gateways.  Microsoft says Lync can serve as the &#8220;glue&#8221; to keep multiple PBX systems working.  Basically, in the past we have had plenty of people who did down-stream deployments of Lync.  What Microsoft wants you to do now is put Lync as an upstream deployment.  And then trunk from Lync to all of those legacy PBX systems.  The end goal of course is, you will find those PBX&#8217;s annoying and just get rid of them and go with Lync 2013.

So those are the major changes I&#8217;ve seen quickly glancing through the docs.  As we find more information we will of course be posting all of it up for the world to see.

&nbsp;

&nbsp;