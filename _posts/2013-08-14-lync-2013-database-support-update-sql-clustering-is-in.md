---
id: 357
title: 'Lync 2013 Database Support Update &#8211; SQL Clustering is in!'
date: 2013-08-14T13:05:06+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=357
permalink: /2013/08/14/lync-2013-database-support-update-sql-clustering-is-in/
st_layout_box:
  - right
categories:
  - Lync Server 2013
---
Thanks to my fellow co-worker and dear Friend Acer for pointing this one out but Microsoft quietly updated the database support page.  It now reads:

Lync Server 2013 supports the user of either SQL mirroring or SQL clustering for each Lync Server database. You can easily set up SQL mirroring with the Topology Builder tool in Lync Server 2013. For SQL failover clustering, you must use SQL Server for setup.

Lync Server 2013 supports SQL clustering topologies for all deployments, including greenfield deployments and organizations that have upgraded from previous versions of Lync Server.

SQL Clustering support is for an active/passive configuration. For performance reasons, the passive node should not be shared by any other SQL instance.

The following support is included:

<li style="color: #000000;font-size: 12pt">
  Two-node failover clustering for the following: <ul>
    <li style="color: #000000;font-size: 12pt">
      Microsoft SQL Server 2012 Standard (64-bit edition). Additionally running the latest service pack is recommended.
    </li>
    <li style="color: #000000;font-size: 12pt">
      Microsoft SQL Server 2008 R2 Standard (64-bit edition). Additionally running the latest service pack is recommended.
    </li>
  </ul>
</li>

  * Up to sixteen-node failover clustering for the following: 
    <li style="color: #000000;font-size: 12pt">
      Microsoft SQL Server 2012 Enterprise (64-bit edition). Additionally running the latest service pack is recommended.
    </li>
      * Microsoft SQL Server 2008 R2 Enterprise database software (64-bit edition). Additionally running the latest service pack is recommended.

Great news today!  <http://technet.microsoft.com/en-us/library/gg398990.aspx>