---
id: 535
title: List all Active Conferences via PowerShell
date: 2013-11-19T14:48:26+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=535
permalink: /2013/11/19/list-all-active-conferences-via-powershell/
st_layout_box:
  - right
panels_data:
  - 'a:0:{}'
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
categories:
  - Lync Server 2013
---
UPDATE: 11/20/2013 &#8211; 10:24 AM.  Fixed an issue on the script.  Sorry everyone!

UPDATE: 09/12/2018 &#8211; Jameson took this script, made it better and posted it to <a href="https://github.com/ImNtReal/SfBScritps/blob/master/Get-CsActiveConferences.ps1" target="_blank" rel="noopener">GitHub</a>.

Let me start by saying that I am NOT even half as good as the great [Mr. Pat Richard](http://www.ehloworld.com/category/powershell) when it comes to PowerShell.  If he is the major league of PowerShell, on my best day I&#8217;m a solid backup player for a little league team.  Therefore the PowerShell that I&#8217;m about to share could be a lot better but think of it as a first step to something better in the future.  If you have any improvements feel free to add them to the comments.

Over the last few weeks I&#8217;ve been thinking about a way to assist a client in the draining of their servers for the purposes of patching.  As it is well documented, the draining process will leave services up and running in a paused state until all active conversations are completed.  The issue I&#8217;ve ran into a few times however is that I&#8217;ve waited over 24 hours and some services (typically the IM MCU) are still up and running.  Often times, this is just a random person who left a conference IM window open on their computer but none the less I don&#8217;t want to bounce the box if there is a legitimate conference going on.

So I started digging around the database (because that is where all the cool stuff happens in Lync) and knowing exactly where all of the active conference are happening decided to start looking for a way to display that information via PowerShell.  I decided the simplest way was to prompt for a pool, look-up the members of that pool and loop through each of the front-end servers.

The SQL is the easy part:

<p style="padding-left: 30px">
  SELECT ActiveConference.ConfId AS &#8216;Conference ID&#8217;, ActiveConference.Locked, Participant.UserAtHost AS &#8216;Participant&#8217;, Participant.JoinTime, Participant.EnterpriseId, ActiveConference.IsLargeMeeting AS &#8216;Large Meeting&#8217; FROM ActiveConference INNER JOIN Participant ON ActiveConference.ConfId = Participant.ConfId
</p>

Each front-end server in the RTCDYN database has an ActiveConference table which contains all of the active conferences running on that particular server.

<img class="alignnone wp-image-536 size-full" src="https://masteringlync.com/wp-content/uploads/2013/11/pic24.png?resize=300%2C195&ssl=1 300w" sizes="(max-width: 523px) 100vw, 523px" data-recalc-dims="1" />

And the Participants table contains all of the active participants for the call with a corresponding conference ID.  Therefore all we need to do is join the two tables together and we have a complete list.  Using this information I went ahead and created this short script:

<img class="alignnone wp-image-541 size-medium" src="https://masteringlync.com/wp-content/uploads/2013/11/pic35.png?w=946&ssl=1 946w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" />

Download Here: [Get-CSActiveConferences](http://masteringlync.com/wp-content/uploads/sites/2/2013/11/Get-CSActiveConferences.txt)

You will need to rename the script to .ps1 from .txt.  Once you have renamed the file, go ahead and run .Get-CsActiveConferences from the Lync Management Shell.  It will prompt you for the name of the pool you want get results from.  Your results will look like:

<img class="alignnone wp-image-544 size-large" src="https://masteringlync.com/wp-content/uploads/2013/11/pic12.png?w=1373&ssl=1 1373w" sizes="(max-width: 800px) 100vw, 800px" data-recalc-dims="1" />

NOTE: You may need to adjust firewall rules if you don&#8217;t allow your local SQL databases to be accessible remotely.  Port 1433 is the default SQL port.

All meetings will be grouped by Conference ID.  By default I&#8217;m showing the Participant, Join Time and if it is a Large Meeting.  You can always edit the PowerShell file to display other information if needed.  If you have any good requests or changes please just pass them along and I&#8217;ll post them.