---
id: 1241
title: CMS Changes in Skype for Business 2015
date: 2016-02-24T16:41:37+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=1241
permalink: /2016/02/24/cms-changes-in-skype-for-business-2015/
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
  - Skype for Business
---
While doing some testing of some advanced/crazy items I need to do in my Skype for Business 2015 deployment, I discovered something, CMS changed from 2013 to 2015.

**What is the Central Management Store (CMS)?**

The CMS is a SQL Database that stores your topology for your SfB Environment.  The contents of your database can be seen in XML format by exporting your configuration (export-csconfiguration) and is replicated to other servers via the FileTransferAgent.

Your CMS contains your topology (what you edit in Topology Builder), Configuration (conference numbers, Oauth information, etc.) and Policies (dial plan, voice, etc.).  All of these are stored as XML inside of the above mentioned SQL Database.  If we connect to our Standard Edition Server that is hosting CMS we will find an XDS Database.  (If you have an Enterprise Edition pool, connect to your backend server.)

<a href="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/02/1-1.png" rel="attachment wp-att-1242"><img class="alignnone size-full wp-image-1242" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/02/1-1.png?resize=216%2C267&#038;ssl=1" alt="1" width="216" height="267" data-recalc-dims="1" /></a>

**CMS Replication**

There are three pieces to CMS Replication.

<span style="text-decoration: underline">Master Replica Agent:</span> A service running on your front-end server(s) that is designated as the CMS Master.

<span style="text-decoration: underline">File Transfer Agent:</span> A service running on your front-end server(s) that is designated as the CMS Master.

<span style="text-decoration: underline">Replica Agent:</span> A service running on all Front-End, Edge, Director, Mediation, TrustedApp Servers, PChat, etc. that contain a replica of the CMS.

How the process works is that the Master Replica Agent makes a copy of the configuration in a data.zip file and is copied into specific folders (to-replica).  The File Transfer Agent then moved that file either via SMB or HTTPS (for edge servers only) to the replicas.  The replica&#8217;s process the file, and create a status.zip file in their own file share.  The File Transfer Agent sees the file, transfers it back to the central file share and the Master Replica Agent processes that it was completed.

If you want the deep dive details of this you should check out this [blog](https://lyncdude.com/2014/09/26/simple-understanding-of-lync-cms/) for more details and pretty pictures explaining it all.

**So What Changed?**

The location of the file share!  And no one told anyone about it.  If you upgraded your Lync 2013 environment to SfB 2015 then you most likely have a bunch of folders in your file share.  However, if you did a clean install or moved your CMS to a new SfB Server it most likely looks like this:

<a href="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/02/2.png" rel="attachment wp-att-1243"><img class="alignnone size-full wp-image-1243" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/02/2.png?resize=800%2C141&#038;ssl=1" alt="2" width="800" height="141" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/02/2.png?w=939&ssl=1 939w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/02/2.png?resize=300%2C53&ssl=1 300w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/02/2.png?resize=768%2C135&ssl=1 768w" sizes="(max-width: 800px) 100vw, 800px" data-recalc-dims="1" /></a>

After a minute of absolute panic that someone had deleted everything for CMS I realized that CMS replication was still working.  So clearly it moved, but to where.

So I fired up Process Monitor and entered these filters.

<a href="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/02/3.png" rel="attachment wp-att-1244"><img class="alignnone size-full wp-image-1244" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/02/3.png?resize=496%2C72&#038;ssl=1" alt="3" width="496" height="72" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/02/3.png?w=496&ssl=1 496w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/02/3.png?resize=300%2C44&ssl=1 300w" sizes="(max-width: 496px) 100vw, 496px" data-recalc-dims="1" /></a>

What I found wasn&#8217;t all that shocking.  First, we see the Master Replica Agent creating the data.zip files.

<a href="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/02/4.png" rel="attachment wp-att-1245"><img class="alignnone size-full wp-image-1245" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/02/4.png?resize=800%2C103&#038;ssl=1" alt="4" width="800" height="103" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/02/4.png?w=972&ssl=1 972w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/02/4.png?resize=300%2C39&ssl=1 300w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/02/4.png?resize=768%2C99&ssl=1 768w" sizes="(max-width: 800px) 100vw, 800px" data-recalc-dims="1" /></a>

Notice the path.  \\server\xds-replica\xds-master\xds-master &#8230;  The CMS has moved from the back-end file share to the RTCReplicaRoot share that is often placed at the root of C.

Next we can see the File Transfer Agent copying the data.zip file to another server.

<a href="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/02/5.png" rel="attachment wp-att-1246"><img class="alignnone size-full wp-image-1246" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/02/5.png?resize=800%2C47&#038;ssl=1" alt="5" width="800" height="47" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/02/5.png?w=858&ssl=1 858w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/02/5.png?resize=300%2C17&ssl=1 300w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/02/5.png?resize=768%2C45&ssl=1 768w" sizes="(max-width: 800px) 100vw, 800px" data-recalc-dims="1" /></a>

And if we let this process play out long enough we will see the Master Replica Agent processing the status.zip files.

<a href="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/02/6.png" rel="attachment wp-att-1247"><img class="alignnone size-full wp-image-1247" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/02/6.png?resize=800%2C66&#038;ssl=1" alt="6" width="800" height="66" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/02/6.png?w=965&ssl=1 965w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/02/6.png?resize=300%2C25&ssl=1 300w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/02/6.png?resize=768%2C64&ssl=1 768w" sizes="(max-width: 800px) 100vw, 800px" data-recalc-dims="1" /></a>

**Why the move?**

So I asked around to find out why this move happened.  The vast majority of people didn&#8217;t even realize that the process had changed from Lync 2013 to Skype for Business 2015.  But after asking enough people it appears this change was to remove a point of failure.  Apparently too many people have been putting their Lync 2010/2013 share on an unsupported file structure (i.e. NetApp) causing massive CMS replication issues for support.