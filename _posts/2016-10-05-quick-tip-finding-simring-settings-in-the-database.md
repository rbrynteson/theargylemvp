---
id: 1288
title: 'Quick Tip: Finding SimRing Settings in the Database'
date: 2016-10-05T16:10:33+00:00
author: Richard Brynteson
layout: post
guid: http://masteringlync.com/?p=1288
permalink: /2016/10/05/quick-tip-finding-simring-settings-in-the-database/
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
  - Lync Server 2010
  - Lync Server 2013
  - Skype for Business
---
In this scenario we have a user who is having his/her phone SimRing to an endpoint (such as their mobile device) yet the client doesn&#8217;t show any SimRing turned on.  How can you verify in the database what is setup for the user.

**Issue**

We will need SQL Management Studio loaded on a server in order to accomplish this.

  1. Connect to the database.  If Lync 2010, go to the backend database.  If Lync 2013/SfB go to the primary front-end server for that user.  You can use the get-csuserpoolinfo to determine which server is primary.
  2. Browse to the server\RTCLocal (if 2013/Sfb) or server\RTC (if 2010)
  3. Find your users resource ID.  Open a new SQL Query window:SELECT [ResourceId] ,[UserAtHost] FROM [rtc].[dbo].[Resource] WHERE UserAtHost LIKE &#8216;rbrynteson@%&#8217;This will return a ResourceID, take note of it.
  4. Now head over and open up the PublishedStaticInstance.  And run this command:SELECT TOP 1000 [PublisherId],[CategoryId] ,[ContainerNum] ,cast(cast([Data] as varbinary(max)) as varchar(max)) as Data FROM [rtc].[dbo].[PublishedStaticInstance] WHERE PublisherId = &#8216;9&#8217; AND [CategoryId] = &#8216;8&#8217;Change PublisherID to the ResourceID that you found above.  The CategoryID should be 8, which comes from the CategoryDef table and refers to routing.Once you do the above, you would see something like this: 
    <a href="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/10/1.png" rel="attachment wp-att-1289"><img class="alignnone wp-image-1289 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/10/1.png?resize=695%2C253&#038;ssl=1" alt="1" width="695" height="253" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/10/1.png?w=695&ssl=1 695w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/10/1.png?resize=300%2C109&ssl=1 300w" sizes="(max-width: 695px) 100vw, 695px" data-recalc-dims="1" /></a></li> 
    
      * You can than take ContainerNum 0 (which is everyone level) and copy/paste the Data column to Notepad.  From there your file is going to look similar to below.  NOTE: I have two values instances of it for comparison.<a href="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/10/2.png" rel="attachment wp-att-1291"><img class="alignnone wp-image-1291 size-column2-1/1" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/10/2-570x292.png?resize=570%2C292&#038;ssl=1" alt="2" width="570" height="292" data-recalc-dims="1" /></a>In the top XML, you will see value=&#8221;simultaneous\_ring&#8221; which means the server believes that SimRing is still enabled.  Your client might not show this, but this is the routing rules it’s going to follow.  In the second XML, you can see what it looks like when I’ve disabled SimRing.  The value should be equal to &#8220;&#8221;.Lastly, you could also see a value=&#8221;enablecf forward\_immediate&#8221; if call forward was enabled.</ol> 
    
    **Solution**
    
    Honestly, there isn&#8217;t a great solution in this case.  If the database becomes screwed up in that it will no longer allow you to set a forward, you may need to remove the user and add them back into the system.
    
    **Unsupported Solution**
    
    NOTE: This is not supported.  I would do this in a lab but not production unless you have your resume up to date, know what you are doing and did I mention be prepared to get a new job if this goes sideways.  Don&#8217;t blame me for doing this in production, do it in a lab.  Clear enough?
    
    I&#8217;ve mentioned this tool before but here goes again.  There is a tool that I found that supports editing Image columns and specifically the XML file within them.  If you download [EMS SQL Studio](http://www.sqlmanager.net/en/products/studio/mssql) (I’ve never used this product before – just found it on the innerwebs) you can see and modify the XML in the data column.  Here you can see my XML file:
    
<img class="alignnone" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/11/pic8.png?resize=658%2C374&#038;ssl=1" alt="" width="658" height="374" data-recalc-dims="1" /> 
    
    You can edit the above XML file you found (if on 2013/SfB &#8211; make sure you are on the primary server).  This will replicate to other servers and magically solved the issue.  In this case, I removed the sim_ring value and set it to &#8220;&#8221;.
    
    &nbsp;