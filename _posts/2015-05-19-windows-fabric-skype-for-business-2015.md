---
id: 1081
title: Windows Fabric + Skype for Business 2015
date: 2015-05-19T21:29:47+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=1081
permalink: /2015/05/19/windows-fabric-skype-for-business-2015/
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
colormag_page_layout:
  - default_layout
image: https://masteringlync.com/wp-content/uploads/sites/2/2015/05/Fabric.png
categories:
  - Featured
  - Skype for Business
---
Back in October 2013 I wrote an article on how [Windows Fabric and Lync Server 2013](http://masteringlync.com/2013/10/29/understanding-how-windows-fabric-works/) interact with one another and now it&#8217;s time to write an in-depth article on what&#8217;s change.

**Overview**

Lync Server 2013 used Windows Fabric v1, Skype for Business 2015 uses Windows Fabric v3, but this wasn&#8217;t always going to be the case.  Initially the thought was Server 2008 R2 was going to support only Fabric v2.0 but somewhere late in the process the key parts of v3.0 (or maybe the whole thing) were ported back to work on 2008 R2.

<table width="50%">
  <tr>
    <td>
      OS Version
    </td>
    
    <td>
      Fabric Version
    </td>
  </tr>
  
  <tr>
    <td>
      Windows 2012 R2
    </td>
    
    <td>
      Fabric v3.0
    </td>
  </tr>
  
  <tr>
    <td>
      Windows 2012
    </td>
    
    <td>
      Fabric v3.0
    </td>
  </tr>
  
  <tr>
    <td>
      Windows 2008 R2
    </td>
    
    <td>
      Fabric v3.0
    </td>
  </tr>
</table>

So although Windows 2008 R2 supports Fabric v3.0 and an in-place upgrade I wouldn&#8217;t do it.  Support long term to me is questionable and when and what CU stops supporting it will be the million dollar question.

_RECOMMENDATION_: If you are running Windows 2008 R2 you should not do an in-place upgrade.  Your best option for high availability will be to do a side-by-side migration to Skype for Business 2015 on new hardware (or virtual machines).

In terms of how Skype and Fabric work together, it&#8217;s a very similar picture from Lync 2013.

<img class="alignnone wp-image-1083 size-full" src="https://masteringlync.com/wp-content/uploads/2015/03/fab1.png?resize=768%2C428&ssl=1 768w" sizes="(max-width: 800px) 100vw, 800px" data-recalc-dims="1" />

What should be obvious is that the basic concepts are the same.  The backend database exists as a location for blob storage for the front-end servers, this allows the front-ends to hydrate their local database when a routing group is started.  The biggest change however is that conferencing data is now written synchronously to the backend database.  This change will allow conferences to resume faster in the event of a failure of a front-end server.

_RECOMMENDATION_: Don&#8217;t make the assumption that because the backend database is primarily used for hydration that it means you can choose a SQL solution that doesn&#8217;t include high availability (HA).  I&#8217;ve had more than one client tell me that their SQL database is going to be &#8220;virtualized&#8221; and therefore they are not going to do a HA solution.  The backend database is still the location for all response groups information &#8211; both static and dynamic &#8211; and the loss of your backend database even for a short period could have a serious negative effect on your call experience.  Don&#8217;t be cheap with SQL.

**Fabric Basics**

The Windows Fabric is installed during the setup of your Skype for Business Server.  If you are upgrading from Lync Server 2013, your Windows Fabric will be upgraded from v1.0 to the appropriate version during the in-place upgrade.

During installation, Windows Fabric creates a local configuration file at C:\ProgramData\Windows Fabric\<server.domain.com>\Fabric\ClusterManifest.current.xml.  This is a location change from Lync Server 2013 which stored the file at C:\Program Files\Windows Fabric\bin\ClusterManifest.current.xml.

<img class="alignnone wp-image-1167 size-full" src="https://masteringlync.com/wp-content/uploads/2015/05/1.png?resize=1024%2C123&ssl=1 1024w" sizes="(max-width: 800px) 100vw, 800px" data-recalc-dims="1" />

One interesting item to be aware of is that the ClusterManifest.current.xml file is generated new each time the Windows Fabric service starts.  Therefore, the xml file should never be manually altered as the changes will simply be overwritten on the next reboot anyways.  It&#8217;s also important to note that all front-end servers use Windows Fabric, even Standard Edition Services.  Fabric is used for much more then the population of routing group information.  Windows Fabric is used by the Lync Storage Services (LYSS), MCU Factory Services and much more.

**Pool Quorum and Routing Group Quorum**

One of the often misunderstand facts of Lync Server 2013 is how Pool Quorum and Routing Group Quorum works.  This hasn&#8217;t really changed drastically from Lync Server 2013 but it&#8217;s important enough to review the process again.

<span style="text-decoration: underline"><em>Pool Quorum</em></span>

<table style="width: 50%">
  <tr>
    <td>
      Total Number of Servers in Pool
    </td>
    
    <td>
      Number of Servers that must be running
    </td>
  </tr>
  
  <tr>
    <td>
      2
    </td>
    
    <td>
      1
    </td>
  </tr>
  
  <tr>
    <td>
      3-4
    </td>
    
    <td>
      Any 2
    </td>
  </tr>
  
  <tr>
    <td>
      5-6
    </td>
    
    <td>
      Any 3
    </td>
  </tr>
  
  <tr>
    <td>
      7
    </td>
    
    <td>
      Any 4
    </td>
  </tr>
  
  <tr>
    <td>
      8-9
    </td>
    
    <td>
      Any 4 of first 7
    </td>
  </tr>
  
  <tr>
    <td>
      10-12
    </td>
    
    <td>
      Any 5 of first 9
    </td>
  </tr>
</table>

In order for a pool to function it must have a sufficient number of servers to maintain quorum.  In the above table we see the breakdown of number of necessary servers to maintain quorum.  In a large pool environment (more than 8 servers in a front-end pool) the order in which servers are introduced into the pool is critical to be aware of.  In Skype for Business the number of upgrade domains is based on the number of servers in your pool and as such quorum becomes dependent on those upgrade domains be available.

_RECOMMENDATION_: Although the table above includes a pool of two servers you should never deploy a two-server Enterprise Pool in Skype for Business (or Lync Server 2013).  Windows Fabric depends on this concept of replicas and desires three replicas of data to be available.  In a two server pool, it is impossible to have a third replica and therefore the pool itself operates much like a Lync 2010 pool, always writing data to the backend database synchronously.  This is an issue in the event of any failure on the backend database will immediately cause your users to drop into limited availability mode.  Unlike a pool of three or more servers, where a temporary loss of backend services are not immediately noticed by end users except for the above mentioned dependency on response group services.

_<span style="text-decoration: underline">Routing Groups</span>_

The routing group is the heart of Windows Fabric in my mind.  In our example below we have three front-end servers: lyncfe00, lyncfe01 and lyncfe02.  When the fabric service (and hence front-end services) start up for the first time they go through a process in which routing groups are assigned to servers. These routing groups than correspond back to users.  So let&#8217;s walk through the database and see where we see the routing group.

<img class="alignnone" src="https://masteringlync.com/wp-content/uploads/2013/10/pic22.png?resize=800%2C283&#038;ssl=1" alt="" width="800" height="283" data-recalc-dims="1" /> 

We can connect to any of the RTCLocal\RTC databases running on the front-end of any server.  Remember, Windows Fabric means we don&#8217;t need to rely on the back-end database so all of this data is sitting on the front-ends.  By looking at the RoutingGroupAssignment table we can see which routing groups are assigned to which front-end servers at a given time.  Therefore in my example, we can see six different routing groups.  When you add more users to a pool the total number of routing groups continues to grow.  The second query is against the FrontEnd table and here we can see that 12040 is lyncfe00.thelab.info server.  As you can see in my lab, of the six routing groups that exist, there are two routing groups assigned to each of the three front-end servers that are currently running.

Users are assigned to these routing groups.  This information can be found in both Active Directory and in the SQL Database.  If we look at the msRTCSIP-UserRoutingGroupId in Active Directory we can see the routing group a user is assigned to in reverse order.

<img class="alignnone" src="https://masteringlync.com/wp-content/uploads/2013/10/pic32.png?resize=534%2C374&#038;ssl=1" alt="" width="534" height="374" data-recalc-dims="1" /> 

As you can see here, the msRTCSIP-UserRoutingGroupID in active directory corresponds to a routing group defined within Skype for Business.  Some of the numbers are reversed from Active Directory to what is seen in the RTCLocal database.  What is important to know is that this routing group ID doesn&#8217;t change once it&#8217;s set (unless your server gets more routing groups because more users are added to the pool).

EXPERT NOTE: When you invoke a pool-failover the routing groups are assigned from PoolA to PoolB.  If you run these same commands you would see the new routing groups assigned to the PoolB servers.  So a users routing group rarely changes once they have been assigned to one.

The routing group information can be found at an user level as well by running the Get-CsUserPoolInfo command.

<img class="alignnone" src="https://masteringlync.com/wp-content/uploads/2013/10/pic42.png?resize=643%2C280&#038;ssl=1" alt="" width="643" height="280" data-recalc-dims="1" /> 

_<span style="text-decoration: underline">Loading the Routing Group</span>_

Now that we understand what the routing group is and where we can see it assigned to a server, why do I see three servers in my user pool information?  The Windows Fabric will always assign three servers to every routing group.  In my test users case, we can see that lyncfe00 (primary), lyncfe01 (active secondary), lyncfe02 (idle secondary).  If we were to look at another user we could potentially see a different order.

When a pool of front-end servers are started primary servers are assigned to each routing group.  Once a server is assigned as a primary server, that server reaches to the backend database and hydrates the user information (contacts, conference, etc.) for all users assigned to that routing group.  Once the primary server has gathered the information it needed, it replicates that information to both the secondary active and secondary idle servers.  Once this process has completed, the front-end service will go from a starting state to a running state.  When a user makes a change (testuser01 add a contact to their buddy list) it is the responsibility of the primary server to write that information into the back-end database and also add that information to both of the secondary servers.  The writes to the other front-end servers are synchronous for all nodes in the routing group &#8211; that is not the case to the backend database.

EXPERT NOTE: Recall, one of the major changes from Lync to Skype for Business is that some information is written to the backend database synchronously, such as conference creation and status information.

In the event a server goes down the secondary active is promoted to primary.  Since the secondary servers have been receiving synchronous changes there is no need for the old secondary server/new primary server to hydrate any information from the back-end database.  If the pool has more than three servers, a new secondary idle server will be assigned to the routing group and that server will hydrate it changes from the new primary server.  It should be noted that hydration of a new server can take anywhere from 15 to 30 minutes.

_<span style="text-decoration: underline">Routing Groups Quorum</span>_

As mentioned previously, routing groups have quorum in addition to pool quorum and as such you must take special care to ensure that routing group quorum is never lost.  Let&#8217;s take a look at the below example to understand this.

<img class="alignnone" src="https://masteringlync.com/wp-content/uploads/2015/05/2.png?resize=687%2C241&#038;ssl=1" alt="" width="687" height="241" data-recalc-dims="1" /> 

In our above example, we can see that server01, server02 and server03 currently host routing group 111111111.  In the event that server01 were to be lost, server02 and server03 would maintain quorum for the replica group and all users would remain connected.  However, if we lost server02 before a third replica was hydrated from the primary replica server the routing group would lose quorum.  In the event that two of the three servers were lost all users assigned to routing group 111111111 would drop into limited functionality mode even though there are a sufficient number of servers to keep pool quorum and users in routing group 222222222 and 333333333 continue to function without any issues.

If all three replicas are lost within a routing group the routing group goes offline and any users who were connected will be immediately disconnected from the Skype for Business Client.

Recovery of a routing group is not as simply as waiting for replicas to be assigned to new servers.  In the event that two of three replicas have failed users will remain in limited availability mode indefinitely unless the server or servers that were assigned the two replicas that are currently marked down are returned to service.  This is because the single remaining replica does not have the authority to assign the routing group to servers that remain up in the pool.  The other option is to reset the pool status and allow all routing groups to be replaced on the remaining front-end server but this process will take all front-end services offline.

_<span style="text-decoration: underline">Pool Startup</span>_

To complicate matters more a pool needs a minimum number of servers to be available to start or to perform a pool reset.

<img class="alignnone" src="https://masteringlync.com/wp-content/uploads/2015/05/4.png?resize=749%2C500&#038;ssl=1" alt="" width="749" height="500" data-recalc-dims="1" /> 

In the event that there are not a sufficient number of servers in the pool the front-end service simply will not start up.  Therefore, if you were in a situation in which two servers were down and you had lost routing group quorum before resetting the pool to return services it would be critical to ensure you had enough servers up in the pool to start services.  A pool reset executed when not all servers are available could result in the entire pool being taken offline.

**Even Number of Servers in Pool**

One of the interesting side affects of the Windows Fabric process is that you need to keep quorum at all times at the pool level.  This gets more interesting if you have an even number of front-end servers.  Anyone who is familiar with clustering in Windows 2003 knows that you need that third member to vote in the cluster process and the Windows Fabric is no different.  So in the event you have an even number of front-end servers the Primary SQL back-end will be added a voting member.

EXPERT NOTE: Only the primary member of a SQL Mirror will vote in the Windows Fabric.  Therefore in a four server pool, if the primary mirror is currently offline your pool is already down a single member and losing two servers will make the pool non-functional.

So let&#8217;s take a moment to explore a four server front-end pool that uses SQL mirroring.  Since it&#8217;s an even number of servers in the pool we know the SQL server is going to be involved in the voting process.  If you had failed over the primary to the mirror for whatever reason and than took down two of your four front-end servers, you would only have two of five members in the Windows Fabric and front-end services would shut down after five (5) minutes on the remaining two front-end servers.  The lesson is simple &#8211; if you have an even number of servers &#8211; you need to make sure you pay attention to where your SQL mirror is.

RECOMMENDATION: If you were looking for a reason to move to SQL Always On instead of Mirror, this is a good reason to sell the need for Enterprise SQL licensing.

**Virtualization**

Using this example again:

<img class="alignnone" src="https://masteringlync.com/wp-content/uploads/2015/05/2.png?resize=687%2C241&#038;ssl=1" alt="" width="687" height="241" data-recalc-dims="1" /> 

Imagine you are hosting  a Skype for Business deployment that has seven front-end servers within a pool.  You may have a mandate from your executive team to reduce the count of physical hosts by utilizing visualization.  Now this scenario is 100% supported but placement of your servers is critical.  Imagine if you distributed your virtual servers in this format among physical hosts.

<img class="alignnone" src="https://masteringlync.com/wp-content/uploads/2015/05/3.png?resize=687%2C272&#038;ssl=1" alt="" width="687" height="272" data-recalc-dims="1" /> 

Here Servers #1 and #2 are hosted on Physical host #1.  Servers #3 and #4 are hosted on Physical host #2.  All remaining servers are on host #3.  If your data-center were to experience a failure of host #1, all users within routing group 11111111 would fall into limited functionality mode immediately.  If you lost physical host #3, all users in routing group 3333333 would immediately disconnect from Skype for Business.  All of this happens even though pool quorum was never lost but routing group quorum was lost.

RECOMMENDATION: If you have to use visualization, only one virtual guest is ever assigned to a physical host.  Better recommendation, don&#8217;t virtualize Skype for Business (or Lync Server 2013).

**Network Equipment**

You can take the above example one step further.  Let&#8217;s suppose you have a physical server for each of your seven servers and Server #1 and Server #2 are plugged into a single switch.  If you lose that one switch, all users in routing group 11111111 would fall into limited functionality mode.  Obviously you can follow this rabbit down the hole very far.  Proper server and network design is critical.

**Conclusion**

Hopefully this helps you understand both Windows Fabric and the differences between version 1.0 and version 3.0 fabric.

&nbsp;