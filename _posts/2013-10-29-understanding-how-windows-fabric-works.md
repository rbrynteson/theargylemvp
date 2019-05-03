---
id: 462
title: Understanding how Windows Fabric Works (with regards to Lync)
date: 2013-10-29T00:50:30+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=462
permalink: /2013/10/29/understanding-how-windows-fabric-works/
st_layout_box:
  - right
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
  - Lync Server 2013
---
So I decided to continue with my &#8220;understanding how&#8221; [series](http://masteringlync.com/2013/10/23/understanding-how-the-backup-service-works/) of posts with one about the Windows Fabric.  The great thing about Windows Fabric is that you shouldn&#8217;t need to do anything and it should just &#8220;work&#8221;.  But sometimes I think it&#8217;s fun to look at how things actually work under the hood.

**Overview**

A picture is worth a thousand words and this picture has been used numerous times at Lync Conference and other Lync Ignite classes and I think it does a great job of giving a high-level view of what Fabric does and why 2013 choose to use it.

<a href="http://masteringlync.com/2013/10/29/understanding-how-windows-fabric-works/pic1-13/" rel="attachment wp-att-463"><img class="alignnone wp-image-463" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/10/pic11.png?resize=600%2C332&#038;ssl=1" alt="pic1" width="600" height="332" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/10/pic11.png?w=932&ssl=1 932w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/10/pic11.png?resize=300%2C166&ssl=1 300w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/10/pic11.png?resize=768%2C424&ssl=1 768w" sizes="(max-width: 600px) 100vw, 600px" data-recalc-dims="1" /></a>

As the picture details, one of the biggest changes the Windows Fabric allowed is to move user services from the back-end database to the front-end servers.  This gives Lync 2013 several advantages in that you can scale further by adding more servers to the pool, relies less on the back-end database, is self-healing as it automatically monitors the state of other servers in the fabric and gives you a decentralized approach to limit the single point of failure.  One of the biggest grips about the addition of the fabric is the three server enterprise edition server recommendation (NEVER do a two server pool!!!) and the [minimum number of servers required for pool functionality](http://technet.microsoft.com/en-us/library/gg412996.aspx).  I think the benefits of fabric outweigh these negatives that some people will point out.  (Note: I believe Lync 2013 suffers from a new limitation and that is SQL Express on the front-end servers but that is a discussion at a later time.)

**The Routing Group**

To me the greatest advantage of using Windows Fabric is how Lync Server 2013 utilizes the back-end database.  In our examples below we will pretend that we have four front-end servers: lyncfe00, lyncfe01, lyncfe02 and lyncfe03.  When the fabric service (and hence front-end services) start up for the first time they go through a process in which routing groups are assigned to servers. These routing groups than correspond back to users.  So let&#8217;s walk through the database and see where we see the routing group.

So we connect to any of the RTCLocalRTC databases running on the front-end of any server.  Remember, Windows Fabric means we don&#8217;t need to rely on the back-end database so all of this data is sitting on the front-ends.

<a href="http://masteringlync.com/2013/10/29/understanding-how-windows-fabric-works/pic2-11/" rel="attachment wp-att-464"><img class="alignnone wp-image-464 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/10/pic22.png?resize=800%2C283&#038;ssl=1" alt="pic2" width="800" height="283" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/10/pic22.png?w=943&ssl=1 943w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/10/pic22.png?resize=300%2C106&ssl=1 300w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/10/pic22.png?resize=768%2C272&ssl=1 768w" sizes="(max-width: 800px) 100vw, 800px" data-recalc-dims="1" /></a>

In this image is two SQL queries.  The first is looking at the RoutingGroupAssignment table.  In that table will display all of the routing groups in this pool.  Therefore in my example, we can see six different routing groups.  When you add more users to a pool the total number of routing groups continues to grow.  The second query is against the FrontEnd table and here we can see that 12040 is lyncfe00.thelab.info server.  As you can see in my lab, of the six routing groups that exist, there are two routing groups assigned to each of the three front-end servers that are currently running.

The next question is what user(s) are assigned to each routing group.  We can find this via PowerShell but where is this information actually stored?  For this, we go to Active Directory.  Looking at testuser01 Active Directory information and the table of RoutingGroups we can see this:

<a href="http://masteringlync.com/2013/10/29/understanding-how-windows-fabric-works/pic3-10/" rel="attachment wp-att-465"><img class="alignnone wp-image-465 size-full" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/10/pic32.png?resize=534%2C374&#038;ssl=1" alt="pic3" width="534" height="374" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/10/pic32.png?w=534&ssl=1 534w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/10/pic32.png?resize=300%2C210&ssl=1 300w" sizes="(max-width: 534px) 100vw, 534px" data-recalc-dims="1" /></a>

As you can see here, the msRTCSIP-UserRoutingGroupID in active directory corresponds to a routing group defined within Lync.  Some of the numbers are reversed from Active Directory to what is seen in the RTCLocal database.  What is important to know is that this routing group ID doesn&#8217;t change once it&#8217;s set (unless your server gets more routing groups because more users are added to the pool).

EXPERT NOTE: When you invoke a pool-failover the Routing Groups are assigned from PoolA to PoolB.  If you run these same commands you would see the new routing groups assigned to the new servers.  So a users routing group shouldn&#8217;t change all that often within Active Directory.

I mentioned we can see this same information via PowerShell as well.  By running a get-csuserpoolinfo we can get the full fabric information.

<a href="http://masteringlync.com/2013/10/29/understanding-how-windows-fabric-works/pic4-9/" rel="attachment wp-att-466"><img class="alignnone wp-image-466 size-full" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/10/pic42.png?resize=643%2C280&#038;ssl=1" alt="pic4" width="643" height="280" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/10/pic42.png?w=643&ssl=1 643w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/10/pic42.png?resize=300%2C131&ssl=1 300w" sizes="(max-width: 643px) 100vw, 643px" data-recalc-dims="1" /></a>

**Loading the Fabric**

Now that we understand what the routing group is and where we can see it assigned to a server, why do I see three servers in my user pool information?  The Windows Fabric will always assign three servers to every routing group.  In my test users case, we can see that lyncfe00 (primary), lyncfe01 (active secondary), lyncfe02 (idle secondary).  If we were to look at another user we could potentially see a different order.

When a pool of front-end servers are started primary servers are assigned to each routing group.  Once a server is assigned as a primary server, that server reaches to the back-end database and hydrates the user information (contacts, conference, etc.) for all users assigned to that routing group.  Once the primary server has gathered the information it needed, it replicates that information to both the secondary active and secondary idle servers.  Once this process has completed, the front-end service will go from a starting state to a running state.  When a user makes a change (testuser01 creates a new conference for example) it is the responsibility of the primary server to write that information into the back-end database and also add that information to both of the secondary servers.  The writes to the other front-end servers are synchronous for all nodes in the routing group &#8211; that is not the case to the back-end database.

In the event a server goes down the secondary active is promoted to primary.  Since the secondary servers have been receiving synchronous changes there is no need for the old secondary server/new primary server to hydrate any information from the back-end database.  Assuming the pool has more than three servers, a new secondary idle server will be assigned to the routing group and that server will hydrate it changes from the new primary server.  It should be noted that hydration of a new server can take anywhere from 15 to 30 minutes.

**Large Pools Risk**

Because of how Windows Fabric works there is a new risk administrators need to be aware of and that is the large pool/simultaneous server failure.  Let&#8217;s take this example.  Your front-end pool contains 12 servers.  You have deployed those 12 servers as virtual machines over four different physical hosts.  Odds are, there will be at least one routing group that is assigned to the three front-end servers running on that one physical host.  What happens to an end user if the physical hosts crashes that contains all three servers assigned to that one routing group?  The end user will immediately disconnect from Lync and will remain disconnected until their routing group gets assigned to new servers within the pool and the routing grounds get hydrated from the back-end database.  My testing has always been hit or miss in this department.  I&#8217;ve had a scenario where I took down all three servers that were assigned to the routing group and it took approximately 30 minutes for the routing groups to come back online.  In another test, I&#8217;ve seen where I have left them down for hours and they never came back online.  The moment I returned one of the servers within the routing group back online, it than immediately took over and assigned two new servers as routing group members.

If all three servers fail at the same time within the routing group than a quorum lost recovery needs to be run on the server.

It get&#8217;s worse.  Since each replica set also has quorum (Primary, Secondary and Secondary Idle) you would never want to lose two servers at the same time. In this scenario, where two of the three servers in a replica set are lost at a given time, your replica set will lose quorum and you will drop into limited functionality mode.  The only recourse is to issue a quorum loss recovery command or get at least one of those two servers back online.

So the recommendation when it comes to virtualization.  Never have more than a single server on a physical host.  (Why virtualize than?  Good question.)

**Windows Fabric Configuration**

There is nothing you need to do as an administrator to configure the Windows Fabric.  However, if you wanted to look at the configuration, you can browse to the C:Program FilesWindows FabricbinClusterManifest.current.  This file is updated every time the Windows Fabric Service starts up so manually changing this file makes no good sense.

**Even Number of Servers**

One of the interesting side affects of the Windows Fabric process is that you need to keep quorum.  This gets more interesting if you have an even number of front-end servers.  Anyone who is familiar with clustering in Windows 2003 you know that you need that third (odd) member to vote in the fabric process and the Windows Fabric is no different.  So in the event you have an even number of front-end servers the Primary SQL back-end will be added a voting member.

IMPORTANT: It&#8217;s ONLY the primary member who is a voting member in the fabric.  So if you are using a SQL mirror and you have failed over from the Primary to the Mirror, your Windows Fabric is one vote down already!

So let&#8217;s take a moment to explore a four server front-end pool that uses SQL mirroring.  Since it&#8217;s an even number of servers in the pool we know the SQL server is going to be involved in the voting process.  If you had failed over the primary to the mirror for whatever reason and than took down two of your four front-end servers, you would only have two of five members in the Windows Fabric and front-end services would shut down after five (5) minutes on the remaining two front-end servers.  The lesson is simple &#8211; if you have an even number of servers &#8211; you need to make sure you pay attention to where your SQL mirror is.

**Do I Need a Back-end Database**

Of course you need the back-end database but because of the Windows Fabric you rely on it much less.  If you have every taken down your back-end DB, you will notice that unlike Lync Server 2010, the Lync client will continue to function with all of it&#8217;s conferencing and contact information.  The Lync Server has a set timeframe that it can &#8220;survive&#8221; without the back-end database.  That default length of time is 30 minutes.  (You can use the Get-CsRegistrarConfiguration cmdlet to change this length &#8211; but it&#8217;s not recommended to go too nuts with the length of time.)

**Never Two Servers**

First, whenever you have fewer than three servers you don&#8217;t have enough servers to complete the Windows Fabric.  Therefore, you don&#8217;t really have a primary, secondary and secondary idle server.  The impact to this is that you no longer rely on the primary server to hydrate the secondary server.  Instead, the pool acts much like Lync Server 2010 did.  Server A and Server B both gets their information directly from the back-end database.  This also means you cannot survive a back-end database failure like you can when you have three servers.  So in the event of a back-end DB failure, the Lync clients will disconnect/drop into limited functionality mode immediately like Lync Server 2010.

Second, if you ever go from two servers to three servers in the pool you will need to reset the entire pool.  This is important to note because unlike 2010 you would NEVER want to add that third server to topology builder during the mid-day because the topology change will immediately invoke the pool reset and all users will be disconnected as the front-end services restart.

**Conclusion**

The key takeaways from this article should be around how the fabric works, how data is loaded from one server to another and a some details about why you never want two (or fewer) servers in a Lync 2013 pool.

&nbsp;

&nbsp;