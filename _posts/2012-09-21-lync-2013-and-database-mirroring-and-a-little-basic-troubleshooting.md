---
id: 82
title: Lync 2013 and Database Mirroring and a little basic troubleshooting
date: 2012-09-21T01:58:27+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=82
permalink: /2012/09/21/lync-2013-and-database-mirroring-and-a-little-basic-troubleshooting/
st_layout_box:
  - right
panels_data:
  - 'a:0:{}'
categories:
  - Lync Server 2013
---
One of the great new features of Lync Server 2013 is the removal of SQL Clustering Support and the addition of SQL Mirroring Support.  You might be asking yourself, a removal of a feature is a good thing?  I believe so.  Even with all of the advances of clustering configuration and setup I believe the complexities of getting clustering working with SQL are high and difficult to maintain.  Often times, getting organizations to get you high performing shared storage can be difficult.  I also believe that SQL mirroring is pretty easy to configure and use.  Now, is clustering dead for sure?  No, people might scream loud enough to get Microsoft to reverse it&#8217;s decision and I know through testing, it absolutely works.

Now with all of these great things in regards to SQL Mirroring there are a few things to note.

First, only SQL Server 2008 R2 and SQL Server 2012 are supported and the two nodes in the SQL Mirror must be the same edition (Standard, Enterprise, etc.).  Second, SQL Server 2012 Always On is not supported as of today, but I believe we will see it added but I don&#8217;t think it will be on RTM.  Third, SQL Mirror requires a third SQL Server to serve as a witness server for automatic failover of your mirrored databases.  This witness must be the same version (2008 R2 or 2012) that is used as the two nodes of the SQL Mirror but does not need to be the same edition.  Also, SQL Express works as that witness node.

Now that we have covered the basics of Database Mirroring there are some basic troubleshooting items that I think everyone should know about.

**Breaking The Mirror**

Sometimes it is necessary to break a SQL Mirror.  What are these scenarios?  Maybe you are going to have a long-term outage between locations.  During Lync TAP upgrades, it was required to break mirroring for database upgrades which could potentially happen during RTM upgrade.  To break a mirror run the following steps:

<a style="line-height: 19px" href="https://i0.wp.com/masteringlync.com/files/2012/09/Database1.png"><img class="alignright wp-image-83 size-full" title="Database1" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2012/09/Database1.png?resize=252%2C224&#038;ssl=1" alt="" width="252" height="224" data-recalc-dims="1" /></a>

First, go to the primary server.  You need to head over to the server who has the  primary owner.  You can tell if you open up Management Studio and look at the databases.  Here you can see all of my databases are Principal, Synchronized which is the owner.

If I create a new SQL Query from Management Studio and execute this command:

ALTER DATABASE <DBName> SET PARTNER OFF

This will immediately break the mirror.  Now of course you will need to do this for all of the databases on the server.

Now, what happens on the far side?  That is great question.  In my experience it breaks the mirror, it drops the database in a state that is disconnected from the mirror but what I couldn&#8217;t do is delete/remove the database from that server.  In order to be able to get databases in a state I could delete them, you will need to run:

ALTER DATABASE DBName SET PARTNER OFF  
RESTORE DATABASE DBName WITH RECOVERY

Again, you need to do this for each of the databases but now you will be able to delete the database.

**Recreate the Mirror**

So now you might want to recreate a mirror later because of a planned breakage or maybe something else simply went wrong.  So you have used either the PowerShell commands or topology builder:

Install-CsMirrorDatabase -ConfiguredDatabases -FileShare &#8220;[\sql101mirror](///\blmvmsql101mirror)&#8221; -SqlServerFqdn &#8220;sql101.domain.com&#8221; -DropExisitingDatabasesOnMirror

and when you run this command, you get this very nasty looking error.

<img class="alignnone wp-image-85 size-medium" title="Database2" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2012/09/Database2-300x267.png?resize=300%2C267&#038;ssl=1" alt="" width="300" height="267" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2012/09/Database2.png?resize=300%2C267&ssl=1 300w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2012/09/Database2.png?w=563&ssl=1 563w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" /> 

What it is basically telling you is that it cannot create a database mirror end-point.  Why might this be true, well most likely there is already an object created for it and you need to get rid of it.  There are two ways to determine if the object exists &#8211; SQL Query or GUI

SELECT e.name, e.protocol\_desc, e.type\_desc, e.role\_desc, e.state\_desc,[<img class="alignright wp-image-86 size-medium" title="Database3" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2012/09/Database3-246x300.png?resize=246%2C300&#038;ssl=1" alt="" width="246" height="300" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2012/09/Database3.png?resize=246%2C300&ssl=1 246w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2012/09/Database3.png?w=252&ssl=1 252w" sizes="(max-width: 246px) 100vw, 246px" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.com/files/2012/09/Database3.png)

t.port, e.is\_encryption\_enabled, e.encryption\_algorithm\_desc,

e.connection\_auth\_desc

FROM   sys.database\_mirroring\_endpoints e JOIN sys.tcp_endpoints t

ON     e.endpoint\_id = t.endpoint\_id;

&nbsp;

Either a search of the GUI or a quick SQL Command will show you that a mirroring_endpoint exists.  Simply right-click to delete this object and re-run your mirroring and you should be successfully.

As I find more SQL Mirroring tricks I&#8217;ll make sure to post.