---
id: 360
title: 'Lync + SQL: should I mirror or should I cluster'
date: 2013-08-14T23:08:02+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=360
permalink: /2013/08/14/lync-sql-should-i-mirror-or-should-i-cluster/
st_layout_box:
  - right
categories:
  - Lync Server 2013
---
With Microsoft&#8217;s [recent change](http://masteringlync.com/2013/08/14/lync-2013-database-support-update-sql-clustering-is-in/) to allow clustering support with Lync Server 2013 the question that everyone will be asking: should I deploy a SQL Mirror or a SQL Cluster.  Although I say the answer to this question is &#8220;it depends&#8221;, I&#8217;ll try to explain why I think SQL Mirror should be and is the better SQL option for Lync.

First, SQL Mirroring is baked into the product.  The configuration and setup of a SQL mirror using topology builder (or the PowerShell commands) is so straight forward I think just about anyone can do it.  No matter how many different &#8220;wizards&#8221; Microsoft throws at the clustering setup and deployment (both Windows and SQL) it&#8217;s never been &#8220;easy&#8221; to do.  I&#8217;ll grant, setting up a SQL Cluster is easier today than it was in 2003, I still don&#8217;t think it &#8220;dummy proof&#8221;.  And the fact you still rely on Windows Clustering to configure and setup adds another layer to it all.  Once you add the Windows cluster to the mix, now you suddenly have to have six different arguments with the storage team about how the disks should really be setup on the server.  And the fact that you don&#8217;t have to use their &#8220;disk is cheap&#8221; SAN solution is usually a plus for you as well.

Second, the &#8220;baking&#8221; of SQL in Lync goes well beyond just the setup.  You have complete control over the SQL Mirror, who currently owns what, movement of the primary/mirror via Lync PowerShell.  Sure, you can do much of the cluster control via PowerShell with SQL 2012 but it&#8217;s simply not the same thing.  As part of the installation of the SQL mirror all of the RTC groups are assigned the necessary permissions to accomplish all of the tasks a Lync administrator would need.  You don&#8217;t have to go fight the SQL team for permission to move the database from node one to two or figure out just the right amount of permissions so you can control the cluster.

Third, speaking of the SQL team, they hate SQL mirrors.  I mean, really hate them.  You might ask yourself, is this a good thing?  And I would say yes!  On more than one occasion I have found the SQL administrator reaction to the SQL mirror requirement as &#8220;I hate mirrors, I won&#8217;t support a mirrored database and if you insist on having a mirrored database than you are responsible for it&#8221;.  And my answer to them is &#8220;fantastic&#8221; 100% of the time.  The SQL Administrator is rarely a help in the deployment and maintenance of Lync.  Instead, they are the ones who typically deploy some bizarre self-made application that breaks everything in Lync, make changes so you can&#8217;t update the database when you try to patch your server or remove every permission from the server so the next time you happen to reboot a front-end server no services start.  I welcome the administration of SQL servers and a mirror typically gives you that.

Lastly, there is simply no compelling reason not deploy the mirror.  I know this is reverse logic but really, what is the overwhelming and advantageous reason an organization small or large should deploy it to a cluster.  The fact that both clustering and mirrors are on the way out the door with SQL and being replaced with SQL Always-On (which by the way is a fancy mirror database) tells you even Microsoft acknowledges the flaws of the SQL Cluster.

So those are my four reasons why I believe the SQL Mirror is the better option and will continue to be my first recommendation to clients when deploying Lync Server 2013.

In the end, I really wish Microsoft would have done a &#8220;we recommend SQL Mirror but support SQL Clustering&#8221; statement instead of just saying they support them both.  Although some would have found it confusing I think it would have been a better solution.

If you have any other reasons feel free to comment or maybe you love SQL clusters that is fine as well &#8230; although I just don&#8217;t know why you would.