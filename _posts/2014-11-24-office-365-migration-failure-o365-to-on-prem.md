---
id: 900
title: 'Office 365 Migration Failure: O365 to On-Prem'
date: 2014-11-24T01:59:48+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=900
permalink: /2014/11/24/office-365-migration-failure-o365-to-on-prem/
st_layout_box:
  - right
categories:
  - Exchange 2013
---
Everyone once in a while I step outside the Lync/Skype for Business world and do some fiddling around with Exchange.  In my home lab, I setup Office 365 integration with my test tenant.  I&#8217;ve been doing all sorts of migrations from Exchange 2013 On-Premise to Office 365 and had no problems.  Today, I wanted to move one of those mailboxes back on-prem and it failed.

While playing around on the Exchange Online, I ran this command to get log information from the O365 side:

Get-MigrationUserStatistics -IncludeReport -Identity user@brynteson.info | fl

When running this, I found this error:

[<img class="alignnone size-full wp-image-901" alt="pic1" src="https://i2.wp.com/masteringlync.com/files/2014/11/pic1.png?resize=601%2C289&#038;ssl=1" width="601" height="289" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/11/pic1.png?w=601&ssl=1 601w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/11/pic1.png?resize=300%2C144&ssl=1 300w" sizes="(max-width: 601px) 100vw, 601px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.com/files/2014/11/pic1.png)

_ERROR MESSAGE: Transient error ResourceUnhealthyException has occurred.  The system will retry._

While doing multiple searches I couldn&#8217;t find a particular fix to this issue.  While searching for transient error message, I found people having issues with their Exchange DAGs.  Everyone led to running this command:

[<img class="alignnone  wp-image-902" alt="pic2" src="https://i0.wp.com/masteringlync.com/files/2014/11/pic2.png?resize=773%2C80&#038;ssl=1" width="773" height="80" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/11/pic2.png?w=966&ssl=1 966w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/11/pic2.png?resize=300%2C31&ssl=1 300w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/11/pic2.png?resize=768%2C80&ssl=1 768w" sizes="(max-width: 773px) 100vw, 773px" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.com/files/2014/11/pic2.png)

This lead to two different possible solutions.  I tried this solution first:

<li value="1">
  Create a new Active Directory group that is named &#8220;ContentSubmitters&#8221;.  Right click on the group, select Security tab and then grant Admistrators and NetworkService full access to the group.
</li>
  1. Force or wait for Active Directory replication.
  2. Restart the following services: 
    <li value="1">
      Microsoft Exchange Search
    </li>
      1. Microsoft Exchange Search Host Controller

I did this, waited for a long time and it didn&#8217;t seem to fix the issue.  So I turned to the second solution for a Failed ContentIndexState and that is to delete the full-text index catalog directory.  Since I&#8217;m an Exchange newb I did some searching to make sure I didn&#8217;t screw up anything.

The steps were easy.

<li value="1">
  Stop the Microsoft Exchange Search and Microsoft Exchange Search Host Controller.
</li>
  1. Go to the directory where the Exchange Database Index is in a failed state.  Find the folder GUID and delete this folder.  Personally, I moved mine to another drive just to make sure I had a copy but I&#8217;m sure I didn&#8217;t need to. 
    [<img class="alignnone size-full wp-image-904" alt="pic4" src="https://i2.wp.com/masteringlync.com/files/2014/11/pic4.png?resize=703%2C64&#038;ssl=1" width="703" height="64" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/11/pic4.png?w=703&ssl=1 703w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/11/pic4.png?resize=300%2C27&ssl=1 300w" sizes="(max-width: 703px) 100vw, 703px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.com/files/2014/11/pic4.png)</li> </ol> 
    
    <li value="3">
      Start the Microsoft Exchange Search Host Controller.  Wait a few minutes and then start the Microsoft Exchange Search service.
    </li>
    
    After these steps, the ContentIndexState changes from Failed to Crawling:
    
    [<img class="alignnone  wp-image-903" alt="pic3" src="https://i0.wp.com/masteringlync.com/files/2014/11/pic3.png?resize=773%2C90&#038;ssl=1" width="773" height="90" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/11/pic3.png?w=966&ssl=1 966w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/11/pic3.png?resize=300%2C35&ssl=1 300w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/11/pic3.png?resize=768%2C90&ssl=1 768w" sizes="(max-width: 773px) 100vw, 773px" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.com/files/2014/11/pic3.png)
    
    I than went back to the Office 365 Portal and attempted my mailbox migration and my migration from Office 365 to On-Prem completed successfully.