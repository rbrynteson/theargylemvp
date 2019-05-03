---
id: 775
title: 'Quick Tip: Downgrading from Lync Licensed/Full to Eval Version'
date: 2014-07-07T20:16:57+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=775
permalink: /2014/07/07/quick-tip-downgrading-from-lync-licensedfull-to-eval-version/
st_layout_box:
  - right
panels_data:
  - 'a:0:{}'
categories:
  - Lync Server 2013
---
Yes, you read that correctly, we are going to downgrade from a fully licensed to an eval copy of Lync Server.  A few things:

First, this ISN&#8217;T SUPPORTED by Microsoft.  So doing this to production would be a really bad thing.  However, if you have Lync in production you most likely want it to be licensed so this wouldn&#8217;t apply to you.

Second, why in the world would you do this?  Easy, Enterprise Voice pilot!  Let&#8217;s say you are an organization that currently has or are thinking about deploying Lync Server to your environment but are not licensed for Enterprise Voice.  Although it&#8217;s really easy to simply click a few buttons in Topology Builder and enable Enterprise Voice for the servers and users, you may not be licensed to do that.  So if you wanted to test out Enterprise Voice, without breaking your Microsoft licensing agreement, you would need to deploy an Eval Pool (180 days) to test out those services.  You are most likely thinking to yourself, who cares, I own Lync and I&#8217;m just going to test it out and although that works for many organizations if you happen to be a large global organization you may not want to test your Microsoft licensing agreement or risk an audit.

So in my case I had a customer who had a valid Lync Server 2013 license for their pilot but they don&#8217;t have Plus (Voice) User CAL&#8217;s.  When we did the install initially we figured we would just install using the licensed copy of Lync they already own under their license agreement.  But we were informed by Microsoft that since we didn&#8217;t own those Plus (Voice) CAL&#8217;s we needed to go back and install the eval copy instead.  Not wanting to lose all of the work of the deployment we go creative.  Here goes:

According to this TechNet article:

<http://technet.microsoft.com/en-us/library/gg521005.aspx>

The process for moving from Eval to Licensed copy is simply running this command:

msiexec.exe /fvomus server.msi EVALTOFULL=1 /qb

This got me to thinking, obviously the fact Lync is licensed comes directly from the server.msi file alone and nothing else in the database or elsewhere.  Using that logic we did the following:

  1. Removed Microsoft Lync Server 2013, Front-End Server
  2. This will give you two warnings.  The first that you need to potentially restart your computer.  A second warning about server activation and you shouldn&#8217;t do the following unless you know what you are doing.  This is the NOT SUPPORTED part.
  3. Reboot the server.
  4. Mount the Eval disks downloaded from the Microsoft Website.
  5. Browse to e:setupamd64setupserver.msi
  6. Re-patch the server to the same CU level it was before.
  7. Start the Front-End Service.
  8. Ran Get-CsServerVersion from PowerShell and it now returns this:[<img class="alignnone wp-image-776 size-full" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/07/pic1.png?resize=714%2C158&#038;ssl=1" alt="pic1" width="714" height="158" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/07/pic1.png?w=714&ssl=1 714w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/07/pic1.png?resize=300%2C66&ssl=1 300w" sizes="(max-width: 714px) 100vw, 714px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.com/files/2014/07/pic1.png)

Again, most likely a pretty rare reason you would need to do this but in case you do, here are the step.