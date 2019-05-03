---
id: 66
title: Understanding Lync 2013 Server Failover
date: 2012-07-26T02:09:40+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=66
permalink: /2012/07/26/understanding-lync-2013-server-failover/
st_layout_box:
  - right
panels_data:
  - 'a:0:{}'
categories:
  - Lync Server 2013
---
Lync 2010 introduced the concept of a backup registrar.  In this scenario, within topology builder, you have the ability to define a backup register and publish that information into the topology.  This was vital for not only the Survivable Branch Appliance but gave users the ability to fall into limited functionality mode in the client.

<img src="https://i1.wp.com/blogs.technet.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-00-66-75-metablogapi/2134.image_5F00_42.png?resize=573%2C182&#038;ssl=1" alt="" width="573" height="182" data-recalc-dims="1" /> 

What happens is the client, upon login, is given both the primary and backup registrar pool during login with the q=0.7 and q=0.3 values – where q=0.7 is the primary server and q=0.3 is your backup server.

<img style="float: right" src="https://i2.wp.com/blogs.technet.com/resized-image.ashx/__size/850x0/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-00-84-94/5873.ChangePoolDB_5F00_Figure12.jpg?resize=194%2C311&#038;ssl=1" alt="" width="194" height="311" align="right" data-recalc-dims="1" /> When client services stop, the client would go into limited functionality mode.  When the client is in this mode, Enterprise Voice functionality would work, but features like configuring Sim Ring, Conferencing and other services that relied on the back-end database are no longer available.

This was very powerful and allowed Lync to be survivable for enterprise voice features.  The problem was, users would lose access to their contacts, conferences wouldn’t work, response groups would not work and bringing these services back online on the far side required significant work to import contacts using the old dbimpexp.exe tool.

Lync 2013 changes all of these behaviors and makes survivability in Lync more competitive with other PBX vendors.

According to the “What’s new article” ([http://technet.microsoft.com/en-us/library/jj204892%28v=ocs.15%29](http://technet.microsoft.com/en-us/library/jj204892%28v=ocs.15%29 "http://technet.microsoft.com/en-us/library/jj204892%28v=ocs.15%29"))

_As in Lync Server 2010, the main high availability (HA) scheme for Lync Server 2013 Preview is based on server redundancy via pooling. If a server running a certain server role fails, the other servers in the pool running the same role take the load of that server. This applies to Front End Servers, Edge Servers, Mediation Servers, and Directors._

_Lync Server 2013 Preview adds new disaster recovery measures by enabling you to pair Front End pools located in two datacenters. If one of the paired pools goes down, an administrator can fail over the users from that pool to the other pool in the pair, to provide continuation of service._ 

_Lync Server 2013 Preview also adds Back End Server high availability. This is an optional topology in which you deploy two Back End Servers for a Front End pool, and set up synchronous SQL mirroring for all the Lync databases running on the Back End Servers. You may choose whether to deploy a witness for the mirror._

Understanding these is important to understand the functionality of server failover.

#1 is a recap of the feature we have today in Lync 2010.  It tells us that when a server within a pool fails, other servers will pick up it’s place.  For example, failure of front-end server #1 could push the AVMCU features that were hosted on front-end server #1 onto front-end server #2.

#2 is the first look at a new feature of Lync Server 2013.  And unfortunately, we are weeks into the public preview of Lync 2013 and I’ve already heard people miss handle the explanation of this feature.  So what is this exactly?  In Lync Server 2013, when you specify a pool as a backup registrar to another pool, this does a lot more than it used to.  [<img class="alignright size-medium wp-image-1046" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2012/07/image_thumb5-300x123.png?resize=300%2C123&#038;ssl=1" alt="image_thumb" width="300" height="123" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2012/07/image_thumb5.png?resize=300%2C123&ssl=1 300w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2012/07/image_thumb5.png?w=314&ssl=1 314w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2012/07/image_thumb5.png)First off, configuration of this option is exactly the same as before within topology.  Here we can see the same configuration option as before.  We can specify automatic failover and failback for voice and again specify the time limits.  This is where items start to change though.  If we take a peak at the services installed on the server, we see something completely new.  The Lync Server Backup Service.  It is important to note, that you must run bootstrapper on the server after you specify a backup registrar.

[<img class="alignnone wp-image-1047 size-full" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2012/07/image_thumb11.png?resize=555%2C36&#038;ssl=1" alt="image_thumb1" width="555" height="36" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2012/07/image_thumb11.png?w=555&ssl=1 555w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2012/07/image_thumb11.png?resize=300%2C19&ssl=1 300w" sizes="(max-width: 555px) 100vw, 555px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2012/07/image_thumb11.png)

So what exactly is this service doing?  According to TechNet, “Lync Server Backup Service provides real-time data replication to keep the pools synchronized”.  So what data are we talking about here?  Pretty much everything.  User information, contacts, conferencing data and more.  Pretty much everything in the database (with the exception of response group information).  What happens if the front-end service is turned off on each server?  And here is where the early reports are slightly confusing.  Some have assumed that if services are offline on one pool, users will automagically failover with everything.  That isn’t quite the case.  Users will failover but have the same limited feature set that they had in the previous version of Lync.

&nbsp;

So this should look very similar to anyone who has used Lync previously.  So now our users have a similar set of features.  I can IM and make/receive phone calls but items like conferencing still aren’t available.  So what does #2 of the new features of Lync 2013 exactly tell us.  An administrator can declare an emergency and fail over the pool to the backup pool.  That is done by using the:

Invoke-CsPoolFailover –PoolFQDN <Pool fqdn> –DisasterMode -Verbose

[<img class="alignnone size-full wp-image-1048" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2012/07/image_thumb21.png?resize=244%2C219&#038;ssl=1" alt="image_thumb2" width="244" height="219" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2012/07/image_thumb21.png)[<img class="alignnone size-medium wp-image-1049" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2012/07/image_thumb31-300x117.png?resize=300%2C117&#038;ssl=1" alt="image_thumb3" width="300" height="117" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2012/07/image_thumb31.png?resize=300%2C117&ssl=1 300w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2012/07/image_thumb31.png?w=352&ssl=1 352w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2012/07/image_thumb31.png)

There is one important note about the above command.  The –DisasterMode is required if the pool (or front-end services) are down.  If you leave that off, you will receive an error in the Management Shell.  The great thing here is this, you could use this command to force a failover to a far side pool so you can perform maintenance on all of the servers.  When the command is completed, the Lync Client will automatically refresh with client and take it out of Limited Functionality Mode.  Contacts, groups, and everything else will come back as they were before.

So what have we learned thus far?  Just because Lync 2013 includes this exciting new feature for DR purposes, there is still some sort of intervention required.  Personally, this is a great decision, because we don’t want users simply flipping between servers because of latency issues.  Second, this completely changes how HA/DR are done today.  How about this HA/DR scenario.  You are a small business and you want to implement some sort of HA/DR.  In the past, you needed to spin an enterprise pool, clustered SQL and much more.  Now, you can spin two Standard Edition servers and point them at each other for failover.

So what happens when your servers are back online and you want to fail-back.  Again, we do this through PowerShell:

Invoke-CsPoolFailback –PoolFQDN <Pool fqdn> –Verbose

[<img class="alignnone size-medium wp-image-1050" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2012/07/image_thumb41-300x61.png?resize=300%2C61&#038;ssl=1" alt="image_thumb4" width="300" height="61" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2012/07/image_thumb41.png?resize=300%2C61&ssl=1 300w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2012/07/image_thumb41.png?w=450&ssl=1 450w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2012/07/image_thumb41.png)

Now, one last interesting item, the failback also has the –disastermode option.  Not 100% sure where the use case for this would be.  Maybe you failed over to the backup pool but not that pool has failed and you are going back to your production pool that is back with all of it’s data?  I guess it could happen, but if you were suffering that much failure I have a gut feeling you have other issues.