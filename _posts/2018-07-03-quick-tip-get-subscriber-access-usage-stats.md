---
id: 1531
title: 'Quick Tip: Get Subscriber Access Usage Stats'
date: 2018-07-03T14:31:01+00:00
author: Richard Brynteson
layout: post
guid: http://masteringlync.com/?p=1531
permalink: /2018/07/03/quick-tip-get-subscriber-access-usage-stats/
colormag_page_layout:
  - default_layout
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
image: https://masteringlync.com/wp-content/uploads/sites/2/2018/07/azure.microsoft.com_.png
categories:
  - Lync Server 2010
  - Lync Server 2013
  - Skype for Business
---
So recently I had a task to figure out how often Subscriber Access was being used in an environment.  Thankfully all of the data you need is in the SQL Database, specifically in the LcsCDR database.

<pre><span style="color: #339966;">/******</span>
<span style="color: #339966;">- Author: Richard Brynteson, T2M</span>
<span style="color: #339966;">- Date: 7/3/2018</span>
<span style="color: #339966;">- Version: 0.9</span>
<span style="color: #339966;">-</span>
<span style="color: #339966;">- Instructions: Set the variables for the Subscriber Access Name and time range</span>
<span style="color: #339966;">******/</span>
<span style="color: #339966;">USE LcsCDR;</span>
<span style="color: #339966;">DECLARE @SAName nvarchar(50);</span>
<span style="color: #339966;">DECLARE @StartTime datetime;</span>
<span style="color: #339966;">DECLARE @EndTime datetime;</span>

<span style="color: #339966;">SET @SAName = 'sa@t2mdev.com'</span>
<span style="color: #339966;">SET @StartTime = '4/1/2018'</span>
<span style="color: #339966;">SET @EndTime = '7/31/2018'</span>

<span style="color: #339966;">SELECT SUM(SessionIdSeq) AS NumSessions, FromUriType, DATEPART(mm, SessionIdTime) as MonthNum, DATEPART(YEAR, SessionIdTime) as YearNum</span>
<span style="color: #339966;">FROM [LcsCDR].[dbo].[SessionDetailsView]</span>
<span style="color: #339966;">WHERE ToUri = @SAName AND InviteTime BETWEEN @StartTime AND @EndTime</span>
<span style="color: #339966;">GROUP BY FromUriType, DATEPART(mm, SessionIdTime), DATEPART(YEAR, SessionIdTime)</span>
<span style="color: #339966;">ORDER BY YearNum, MonthNum, FromUriType</span></pre>

So in this script, all you need to do is to update the three variables to the items to your specific needs and run the script connected to the CDR Database.

The expected output would be:

<img class="alignnone size-full wp-image-1532" src="https://masteringlync.com/wp-content/uploads/2018/07/Output.png?resize=369%2C167&#038;ssl=1" alt="" width="369" height="167" /> 

The output will show you Subscriber Access number of calls separated by PSTN and Client calls into the SA Service.

&nbsp;