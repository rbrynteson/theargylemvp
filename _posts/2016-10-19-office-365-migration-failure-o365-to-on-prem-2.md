---
id: 1302
title: 'Office 365 Migration Failure: O365 to On-Prem'
date: 2016-10-19T02:19:57+00:00
author: Richard Brynteson
layout: post
guid: http://masteringlync.com/?p=1302
permalink: /2016/10/19/office-365-migration-failure-o365-to-on-prem-2/
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
  - Exchange 2010
  - Exchange 2013
---
Everyone once in a while I step outside the Lync/Skype for Business world and do some fiddling around with Exchange.  In my home lab, I setup Office 365 integration with my test tenant.  I&#8217;ve been doing all sorts of migrations from Exchange 2013 On-Premise to Office 365 and had no problems.  Today, I wanted to move one of those mailboxes back on-prem and it failed.

While playing around on the Exchange Online, I ran this command to get log information from the O365 side:

_Get-MigrationUserStatistics -IncludeReport -Identity user@brynteson.info | fl_

When running this, I found this error:

<a href="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/10/1-2.png" rel="attachment wp-att-1303"><img class="alignnone wp-image-1303 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/10/1-2.png?resize=601%2C290&#038;ssl=1" alt="1" width="601" height="290" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/10/1-2.png?w=601&ssl=1 601w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/10/1-2.png?resize=300%2C145&ssl=1 300w" sizes="(max-width: 601px) 100vw, 601px" data-recalc-dims="1" /></a>

ERROR MESSAGE: Transient error ResourceUnhealthyException has occurred.  The system will retry.

While doing multiple searches I couldn&#8217;t find any particular fix to this issue.  While searching for transient error message, I found people having issues with their Exchange DAGs.  Everyone led to running this command:

<a href="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/10/2-1.png" rel="attachment wp-att-1304"><img class="alignnone wp-image-1304" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/10/2-1.png?resize=600%2C63&#038;ssl=1" alt="2" width="600" height="63" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/10/2-1.png?w=966&ssl=1 966w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/10/2-1.png?resize=300%2C31&ssl=1 300w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/10/2-1.png?resize=768%2C80&ssl=1 768w" sizes="(max-width: 600px) 100vw, 600px" data-recalc-dims="1" /></a>

This lead to two different possible solutions.  I tried this solution first:

  1. Create a new Active Directory group that is named &#8220;ContentSubmitters&#8221;.  Right click on the group, select Security tab and then grant Admistrators and NetworkService full access to the group.
  2. Force or wait for Active Directory replication.
  3. Restart the following services: 
      1. Microsoft Exchange Search
      2. Microsoft Exchange Search Host Controller

I did this, waited for a long time and it didn&#8217;t seem to fix the issue.  So I turned to the second solution for a Failed ContentIndexState and that is to delete the full-text index catalog directory.  Since I&#8217;m an Exchange newb I did some searching to make sure I didn&#8217;t screw up anything.

The steps were easy.

  1. Stop the Microsoft Exchange Search and Microsoft Exchange Search Host Controller.
  2. Go to the directory where the Exchange Database Index is in a failed state.  Find the folder GUID and delete this folder.  Personally, I moved mine to another drive just to make sure I had a copy but I&#8217;m sure I didn&#8217;t need to. 
    <a href="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/10/3.png" rel="attachment wp-att-1305"><img class="alignnone wp-image-1305 size-full" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/10/3.png?resize=696%2C68&#038;ssl=1" alt="3" width="696" height="68" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/10/3.png?w=696&ssl=1 696w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/10/3.png?resize=300%2C29&ssl=1 300w" sizes="(max-width: 696px) 100vw, 696px" data-recalc-dims="1" /></a></li> 
    
      * Start the Microsoft Exchange Search Host Controller.  Wait a few minutes and then start the Microsoft Exchange Search service.</ol> 
    
    After these steps, the ContentIndexState changes from Failed to Crawling:
    
    <a href="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/10/4.png" rel="attachment wp-att-1306"><img class="alignnone wp-image-1306" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/10/4.png?resize=602%2C71&#038;ssl=1" alt="4" width="602" height="71" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/10/4.png?w=966&ssl=1 966w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/10/4.png?resize=300%2C35&ssl=1 300w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/10/4.png?resize=768%2C91&ssl=1 768w" sizes="(max-width: 602px) 100vw, 602px" data-recalc-dims="1" /></a>
    
    I than went back to the Office 365 Portal and attempted my mailbox migration and my migration from Office 365 to On-Prem completed successfully.