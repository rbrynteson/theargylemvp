---
id: 444
title: Understanding how the Backup Service Works
date: 2013-10-23T22:08:48+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=444
permalink: /2013/10/23/understanding-how-the-backup-service-works/
st_layout_box:
  - right
panels_data:
  - 'a:0:{}'
categories:
  - Lync Server 2013
---
There are lots of posts that describe how to configure and setup pool pairing including my own [description](http://masteringlync.com/2012/07/26/understanding-lync-2013-server-failover/).  But what I haven&#8217;t seen is a good description of how the backup service works or what is happening behind the scenes.  So in this post I&#8217;ll attempt to walk through the process, what is happening and some of the under the hood that I could discover through investigation.

**The Backup File Share**

The heart of the backup process is the file share.  This will be located in the same location as your normal Lync file share.  So in my case, I have two standard edition servers in a pool-pairing relationship.  Here we can see the contents of that folder:

<a href="http://masteringlync.com/2013/10/23/understanding-how-the-backup-service-works/untitled/" rel="attachment wp-att-447"><img class="alignnone  wp-image-447" src="https://i1.wp.com/masteringlync.com/files/2013/10/Untitled.png?resize=576%2C180&#038;ssl=1" alt="Untitled" width="576" height="180" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/10/Untitled.png?w=720&ssl=1 720w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/10/Untitled.png?resize=300%2C94&ssl=1 300w" sizes="(max-width: 576px) 100vw, 576px" data-recalc-dims="1" /></a>

Here we see four folders.

**CentralMgmt** is going to contain the files necessary for the backup and replication of the Central Management Store.  Two items of note here.  First, if the servers in your pool pairing relationship are not the CMS server you will not see this folder.  Second, replication of CMS for the purposes of backup is completely different than CMS replication that occurs throughout Lync.  The Master Replica and Replica Agents are responsible for keeping the RTCLocalxds database up to date.  The backup service is responsible for keeping the RTCxds database up to date.  Presumably these two databases are nearly the same in contents (the RTCxds since it can take over as CMS for the organization will have more details about replica&#8217;s, etc.) but they server very different purposes.

**ConfServices** are for the replication of conference data from the file share.  When an individual uploads an attachment into the conference we want to make sure that same data is available on the far side in the event of a pool failover.  This is in particular helpful for those individuals who pre-load content.

**UserServices** are for the replication of buddy lists, conference directory ID and other user services information.

You will also find a **TaskAssignment**.  I cannot find any purpose of the TaskAssignment folder as it&#8217;s contents never seem to change.  What is located in that folder is a text file with the servers name listed.  Occasionally a Temp folder will also appear and it is exactly what you would expect it to be used for.

Inside of each of these folders is a Cookie and Data folder.  The Cookie appears to keep track of when the last time data was sent and received.  The data folder contains the actual data being replicated.

**File Copy Process**

Similar to the CMS replication process, the actual process of copying data from PoolA to PoolB is via an SMB file transfer.  What is different however, is that it appears the backup service itself is responsible for both the file copy and consumption of data.

Typically the folder structure will be empty in the data folder but in an attempt to capture the data let&#8217;s do some experiments.  In my lab the servers are lyncfe03 and lyncfe04.  So we will stop the backup service on lyncfe04 and invoke a backup sync.

Some observations:

1. After the invoke, the LyncFE03 server copies the file from FE03 to FE04.  Since the backup service is stopped the file share isn&#8217;t being actively checked for files/changes so they remain in place.  But you can see that LyncFE03 needs permissions to write directly into the LyncFE04 file share.

<a href="http://masteringlync.com/2013/10/23/understanding-how-the-backup-service-works/pic2-10/" rel="attachment wp-att-449"><img class="alignnone wp-image-449 size-full" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/10/pic21.png?resize=712%2C191&#038;ssl=1" alt="pic2" width="712" height="191" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/10/pic21.png?w=712&ssl=1 712w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/10/pic21.png?resize=300%2C80&ssl=1 300w" sizes="(max-width: 712px) 100vw, 712px" data-recalc-dims="1" /></a>

2. The PresenceFocusCookie folder also has a cookie.zip file as well.  Exacting those files reveals a series of folders and XML files for consumption into the backup service.  Based on viewing the contents of the XML file it doesn&#8217;t appear anything is written in &#8220;plain text&#8221; and somehow the data appears obfuscated to some degree.

<a href="http://masteringlync.com/2013/10/23/understanding-how-the-backup-service-works/pic4-8/" rel="attachment wp-att-451"><img class="alignnone wp-image-451 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/10/pic41.png?resize=800%2C279&#038;ssl=1" alt="pic4" width="800" height="279" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/10/pic41.png?w=901&ssl=1 901w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/10/pic41.png?resize=300%2C105&ssl=1 300w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/10/pic41.png?resize=768%2C268&ssl=1 768w" sizes="(max-width: 800px) 100vw, 800px" data-recalc-dims="1" /></a>

**Status of Backup**

Based on the investigation of the backup service it&#8217;s critical that when you use the get-csbackupservicestatus command that you run it on both pools involved in the pair.  For example, with my backup services stopped on lyncfe04 still, I get the following output.

<a href="http://masteringlync.com/2013/10/23/understanding-how-the-backup-service-works/pic3-9/" rel="attachment wp-att-450"><img class="alignnone wp-image-450 size-large" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/10/pic31-1024x275.png?resize=800%2C215&#038;ssl=1" alt="pic3" width="800" height="215" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/10/pic31.png?resize=1024%2C275&ssl=1 1024w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/10/pic31.png?resize=300%2C81&ssl=1 300w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/10/pic31.png?resize=768%2C206&ssl=1 768w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/10/pic31.png?w=1084&ssl=1 1084w" sizes="(max-width: 800px) 100vw, 800px" data-recalc-dims="1" /></a>

We can see the first command is the successful output of the status of lyncfe03.  Here we can see the export status (export to Lync FE04) is in a steady state.  And the import status (import of Lync FE04 data) is normal.  What is doesn&#8217;t tell me is that the LyncFE04 server has actually done anything with that information.  The second command which bleed red all over the screen is the fact that the backup service is down.  Once I start the backup service again on LyncFE04 and allow it to watch/consume data again the backup.zip file disappears.

Back on LyncFE03 however, the PresenceFocusCookie folder was empty when I did the invoke.  Once the services started again, the cookie file is back.  So it appears as though the servers write a file back.

**Configuration of Backup**

One item to make sure to pay some attention to is the actual configuration of your backup service.  You may want to refine this depending on what your WAN bandwidth that is available between location.  A get-csbackupserviceconfiguration reveals:

<a href="http://masteringlync.com/2013/10/23/understanding-how-the-backup-service-works/pic5-7/" rel="attachment wp-att-452"><img class="alignnone wp-image-452 size-full" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/10/pic51.png?resize=476%2C150&#038;ssl=1" alt="pic5" width="476" height="150" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/10/pic51.png?w=476&ssl=1 476w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/10/pic51.png?resize=300%2C95&ssl=1 300w" sizes="(max-width: 476px) 100vw, 476px" data-recalc-dims="1" /></a>

Here we can see that by default, your backup service will sync information every two minutes.  Additionally, it has a maximum conference size of 100Mb.  This could be important if your end users have a habit of uploading large files to your meetings and you want to ensure you don&#8217;t crush your WAN.

**Conclusion**

So what did we learn within this testing.

  * The backup service runs as a service on front-end pools when pool pairing is enabled.
  * This service is both actively copying data and consuming data
  * The backup service appears to track changes to the blob store in the SQL database and writes that information into files on the far side server
  * The backup service is going to monitor its own file share and write that information back into its own database.  Presumably, if you had an Enterprise Edition pool, the windows fabric process would update the remaining servers (I&#8217;ll do some more investigation to verify).
  * Remember, that you can configure all of the backup settings if you want.  So outside of the initial sync, the process isn&#8217;t overly heavy on the WAN (with maybe the exception of what could be conferencing data).