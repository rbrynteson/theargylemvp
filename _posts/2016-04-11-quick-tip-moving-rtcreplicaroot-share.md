---
id: 1256
title: 'Quick Tip: Moving RTCReplicaRoot Share'
date: 2016-04-11T20:55:17+00:00
author: Richard Brynteson
layout: post
guid: http://masteringlync.com/?p=1256
permalink: /2016/04/11/quick-tip-moving-rtcreplicaroot-share/
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
  - Skype for Business
---
Let&#8217;s start by issuing the following disclaimers.

**Warning #1. ** This is not supported in production environments.  You should use at your own risk and should test/validate before implementing.  I&#8217;m not responsible for anything you break.

**Warning #2.**  This procedure has only been tested on Lync 2013 and Skype for Business 2015 servers.  This has not been tested on 2010.

**Warning #3. ** This procedure has not been tested on the CMS Master in Skype for Business 2015.  You should read [this](http://masteringlync.com/2016/02/24/cms-changes-in-skype-for-business-2015/) to find out why deleting this folder could be the end of the world (or your employment).

<span style="text-decoration: underline"><strong>What&#8217;s the Issue</strong></span>

When you install Skype for Business (or Lync 2013/2010), where the installer puts your RTCReplicaRoot folder can be fairly random looking in nature.  There is actually a full procedure of which drive it goes too based on the total number of drives available and and which drive you select to install the program files itself but regardless to say it goes to the wrong place sometimes.

In my research, I found two other posts (<a href="http://www.confusedamused.com/notebook/moving-the-lync-rtcreplicaroot-folder-drive/" target="_blank">here </a>and <a href="http://ucken.blogspot.com/2012/04/resetting-lync-cms-replication.html" target="_blank">here</a>) that detailed how to move it but in each case it required robocopy and other steps I didn&#8217;t want to take and weren&#8217;t 100% successful in my testing in the lab.

<span style="text-decoration: underline"><strong>Move the Share</strong></span>

  1. Take a backup of your CMS just in case.  export-csconfiguration and export-cslisconfiguration for good measure.  Again, if you aren&#8217;t doing this on your CMS master it shouldn&#8217;t matter but just in case.
  2. Stop the Replica Replicator Agent service.  
    <a href="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/04/1.png" rel="attachment wp-att-1257"><img class="alignnone size-full wp-image-1257" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/04/1.png?resize=767%2C149&#038;ssl=1" alt="1" width="767" height="149" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/04/1.png?w=767&ssl=1 767w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/04/1.png?resize=300%2C58&ssl=1 300w" sizes="(max-width: 767px) 100vw, 767px" data-recalc-dims="1" /></a>  
    In my case, I was doing this on the pool paired server.  So this is the backup pool and CMS was not active on this server but that is why you see the other CMS related services.  If you are doing this scenario, you should stop file transfer agent and master replica agent as well for good measure.
  3. Go to the file location of your RTCReplicaRoot file share and delete it.  If you want to make a copy of it, feel free too, but I just straight up removed it.  You will need to take ownership of the folder to do this.
  4. Open regedit and go to:  
    HKEY\_LOCAL\_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Installer\UserData\S-1-5-18\Components\56E4CF11020CD1D4CB4C9207F2558EB4  
    HKEY\_LOCAL\_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Installer\UserData\S-1-5-18\Components\5D0DD2910359E3F4C9DB7363485EFAF3In each instance, update the existing REG_SZ value to the location where you want the RTCReplicaRoot to be installed to.  In my case, I&#8217;m moving from E:\RtcReplicaRoot to C:\RtcReplicaRoot.  
    <a href="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/04/2.png" rel="attachment wp-att-1258"><img class="alignnone size-full wp-image-1258" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/04/2.png?resize=800%2C78&#038;ssl=1" alt="2" width="800" height="78" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/04/2.png?w=1000&ssl=1 1000w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/04/2.png?resize=300%2C29&ssl=1 300w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/04/2.png?resize=768%2C75&ssl=1 768w" sizes="(max-width: 800px) 100vw, 800px" data-recalc-dims="1" /></a>
  5. Still in RegEdit, go to HKEY\_LOCAL\_MACHINE\SYSTEM\CurrentControlSet\Services\REPLICA and update the ImagePath.  This is a really long string that will include the program files but in the middle of the string you will see the replicaRootDir location.  Update to your new location. 
    <a href="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/04/3.png" rel="attachment wp-att-1259"><img class="alignnone size-full wp-image-1259" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/04/3.png?resize=613%2C321&#038;ssl=1" alt="3" width="613" height="321" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/04/3.png?w=613&ssl=1 613w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/04/3.png?resize=300%2C157&ssl=1 300w" sizes="(max-width: 613px) 100vw, 613px" data-recalc-dims="1" /></a>  
    &#8220;D:\Lync 2013\Server\Replica Replicator Agent\ReplicaReplicatorAgent.exe&#8221; -s /replicaRootDir:**C:\RtcReplicaRoot\** /sqlInstance:(local)\rtclocal /sqlDatabase:xds</li> 
    
      * Go to Programs and Features and find Skype for Business Core Components and do a repair.  The process of doing the repair will see that the folder is missing and recreate it for you with all of the correct permissions.
      * Go back to services and restart the Replica service.  You may find CLS is disabled as well, feel free to start that back up.</ol> 
    
    After about 5 minutes, you will find replica is in sync again and all is good.
    
    &nbsp;