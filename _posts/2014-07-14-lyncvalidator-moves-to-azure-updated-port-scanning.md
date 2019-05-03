---
id: 780
title: LyncValidator Moves to Azure / Updated Port Scanning
date: 2014-07-14T01:25:43+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=780
permalink: /2014/07/14/lyncvalidator-moves-to-azure-updated-port-scanning/
st_layout_box:
  - right
panels_data:
  - 'a:0:{}'
categories:
  - Lync Server 2010
  - Lync Server 2013
---
It&#8217;s been quiet on the Lync Validator front for a while but it&#8217;s not because we have been slacking off or no one is using the site.  A quick peek at the stats show that we have scanned nearly 4000 ports, nearly 8000 DNS checks and are closing in on 1,000 saved deployments.  And I promise, it&#8217;s just going to get better.

**What&#8217;s New?**

So there are a few changes that are worth mentioning to the application and are going to be featured here.

_Services In Azure_

From the very start we have been hosting the SQL Backend of LyncValidator in Azure but with this update comes the front-end application as well.  If you have never heard of Azure (and apparently you are living under a rock but have internet to read this post), I believe it might be the single best collection of online tools ranging from SQL, Web Hosting and much more.  I would love to say that I&#8217;m getting paid to make such an endorsement but sadly I am not.  What this change does mean however is that we will increase our uptime from the server that was being hosted in Azure instead of my house.

[<img class="alignnone wp-image-781 size-full" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/07/pic11.png?resize=800%2C186&#038;ssl=1" alt="pic1" width="800" height="186" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/07/pic11.png?w=919&ssl=1 919w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/07/pic11.png?resize=300%2C70&ssl=1 300w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/07/pic11.png?resize=768%2C179&ssl=1 768w" sizes="(max-width: 800px) 100vw, 800px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.com/files/2014/07/pic11.png)

_UDP Service Checks_

Up to this point, the LyncValidator was only doing a single UDP check for service availability.  With this update, we introduce the ability to do a UDP Service check against the Edge Server.  Checking if a UDP port is open isn&#8217;t as simple as doing a port query, like it is with TCP.  Instead, UDP is a connectionless protocol which means in order to check something, we need to send the application a request, wait for a reply and inspect the reply coming from the query.

In order to accomplish this, I wrote a quick web service which took a request to allocate a media port and passed it to the edge server.  The Edge Server rejects my request as I don&#8217;t have the proper permission to do an allocate request but the fact that I get a rejected message back from the server confirms that not only is port 3478 listening but that the AV Service is running on the server.

_Bug Fixes & Improvements_

Fixed two situations in which the HOSTS file creation for the edge server would include only the pool names but not the front-end server names themselves.  Although not strictly required I&#8217;ve seen plenty of situations where the edge server has issues if the pool name isn&#8217;t in the HOSTS file so we will add it now.  Also, updated the reverse proxy HOSTS file to include the external web services name as some reverse proxy solutions (i.e. IIS/ARR) will require that.

But don&#8217;t worry &#8211; over the coming months there will be even more changes.