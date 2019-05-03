---
id: 1157
title: Call Quality Dashboard Config/Use
date: 2015-05-05T23:39:06+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=1157
permalink: /2015/05/05/call-quality-dashboard-configuse/
panels_data:
  - 'a:0:{}'
categories:
  - Skype for Business
---
**Overview**

The Call Quality Dashboard (CQD) is a new feature that allows you to have a new view of your CDR/QoE information.  Unlike the existing Monitoring Server, this feature doesn&#8217;t require or rely on SQL Reporting Servers (SRS) but rather used the SQL SAAS Cube, Archive Database and a web portal.

<img class="alignnone" src="https://i1.wp.com/technet.microsoft.com/dynimg/IC797716.png?resize=603%2C376&#038;ssl=1" alt="" width="603" height="376" data-recalc-dims="1" /> 

EXPERT NOTE: Do not confuse the Skype for Business Archive database that is used as part of CQD offering.  The archive database is a copy of your existing archiving data into another database.  This database is used as the back end database used by the SAAS Cube.

The software ships as a separate download then the main SfB Server and is available on the Microsoft Download Page [here](http://www.microsoft.com/en-us/download/details.aspx?id=46916).

It is important to know that you should deploy a separate server for the purposes of CQD and not attempt to re-provision another server as the performance hit of SAAS can be significant.

**Setup/Install**

The setup and configuration of the server is well established [here](https://technet.microsoft.com/en-us/library/mt126252.aspx).

It boils down to a few main steps:

**1) Install IIS Pre-Reqs**

add-windowsfeature Web-Server, Web-Static-Content, Web-Default-Doc, Web-Asp-Net, Web-Asp-Net45, Web-Net-Ext, Web-Net-Ext45, Web-ISAPI-Ext, Web-ISAPI-Filter, Web-Http-Logging, Web-Url-Auth, Web-Windows-Auth, Web-Mgmt-Console

**2) Install SQL Server**

<img class="alignnone" src="https://i1.wp.com/technet.microsoft.com/dynimg/IC797717.png?resize=334%2C445&#038;ssl=1" alt="" width="334" height="445" data-recalc-dims="1" /> 

3) Product Install

This really boils down to three main areas within the configuration.

[<img class="alignnone size-full wp-image-1158" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/05/a.png?resize=469%2C419&#038;ssl=1" alt="a" width="469" height="419" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/05/a.png?w=469&ssl=1 469w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/05/a.png?resize=300%2C268&ssl=1 300w" sizes="(max-width: 469px) 100vw, 469px" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/05/a.png)

Here we indicate the the location that stores our existing Lync QoE Data.  Second we specify the instance name of the QoE Archive server.  Again, this is the server you just installed SQL Server and Analysis Server onto.  You also must pick a location where to store the files.  These should NOT be on your C (system) drive.  The partition (single vs multiple) is based on if you installed Standard vs Enterprise.  If you have a large amount of data you will see a significant performance increase by selecting multiple partitions/Enterprise SQL.  Lastly, you must specify a username and password that has the correct permissions.  In this instance it is read to Lync QoEMetric database and account that can login to the QoE Archive server (the one we are creating now).

[<img class="alignnone size-full wp-image-1159" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/05/b.png?resize=467%2C374&#038;ssl=1" alt="b" width="467" height="374" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/05/b.png?w=467&ssl=1 467w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/05/b.png?resize=300%2C240&ssl=1 300w" sizes="(max-width: 467px) 100vw, 467px" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/05/b.png)

On the Cube Configuration page you will specify the instance name of where the QoE Archive database is (NOT YOUR QoEMetrics Database).  You also need to specify which server will run the cube analysis.  In a larger deployment you might split the Archive DB and Cube to different servers.  Lastly, you need to enter a user with the correct permissions.  In this instance it is a user that will run permissions, read/write on the QoE Archive database.

[<img class="alignnone size-full wp-image-1160" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/05/c.png?resize=468%2C372&#038;ssl=1" alt="c" width="468" height="372" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/05/c.png?w=468&ssl=1 468w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/05/c.png?resize=300%2C238&ssl=1 300w" sizes="(max-width: 468px) 100vw, 468px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/05/c.png)

On the Portal Configuration page you must specify the QoE Archive SQL Server (AGAIN, NOT THE QoEMetrics Server), Cube Analysis and Repository Server.  This can be the same server in all three instances if you are doing a single server deployment.  Additionally you must specify an account that will run as the portal user.  Setup will create a login security principal to QoE Archive database (with read privilege), a login security principal to Repository database (with read and write privilege) , and a member in QoERole (with full control privilege) for the Cube.

EXPORT NOTE: The installation recommends that you use three different accounts for access.  Clearly it is possible to use a single account.  Outside of a strict security concern there isn&#8217;t a technical reason one account won&#8217;t work.

**Using the Portal**

At this point in time your portal is configured and should work.  There are steps for configuring debug mode on TechNet (which you should do) and limiting access on the server.  To access the server, simply browse to the URL (http://server.domain.com/CDQ/).  You should see something very similar to this.

[<img class="alignnone size-medium wp-image-1161" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/05/d-300x139.png?resize=300%2C139&#038;ssl=1" alt="d" width="300" height="139" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/05/d.png?resize=300%2C139&ssl=1 300w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/05/d.png?resize=768%2C355&ssl=1 768w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/05/d.png?resize=1024%2C474&ssl=1 1024w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/05/d.png?w=1600&ssl=1 1600w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/05/d.png)

**Troubleshooting**

For the most part I have not run into many problems.  You can check the health of your system by viewing the health page at http://server.domain.com/CQD/#/Health .

The largest issue I&#8217;ve seen is when I&#8217;ve needed to re-install the product because of corruption or upgrade.  When attempting to re-install the product you can select to reuse the same QoE Archive database but on the Cube Configuration page you will be prompted that a QoECube already exists and you cannot continue.

To solve this problem:

1) Launch SQL Management Studio

2) Select Analysis Server and connect to your database.

[<img class="alignnone size-full wp-image-1162" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/05/11.png?resize=238%2C166&#038;ssl=1" alt="1" width="238" height="166" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/05/11.png)

Don&#8217;t pick Database Engine.  You won&#8217;t find the object and the installer will continue to fail.

3) Select the QoECube object and delete.

[<img class="alignnone size-medium wp-image-1163" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/05/21-230x300.png?resize=230%2C300&#038;ssl=1" alt="2" width="230" height="300" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/05/21.png?resize=230%2C300&ssl=1 230w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/05/21.png?w=287&ssl=1 287w" sizes="(max-width: 230px) 100vw, 230px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/05/21.png)

In the coming days I&#8217;ll be posting some neat tricks and how-to articles on using the Cube.  It&#8217;s a powerful application that allows you to create your own reports.  It doesn&#8217;t have nearly the feature set of paid products like EventZero or IRPrognosis but it certainly can insight then the standard reports.

&nbsp;

&nbsp;