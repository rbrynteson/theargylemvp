---
id: 1520
title: Leaving a Microsoft Teams Team Forever
date: 2018-05-16T13:20:10+00:00
author: Richard Brynteson
layout: post
guid: http://masteringlync.com/?p=1520
permalink: /2018/05/16/leaving-a-microsoft-teams-team-forever/
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
image: https://masteringlync.com/wp-content/uploads/sites/2/2017/12/Microsoft-Teams.jpg
categories:
  - Microsoft Teams
---
If you were like me when guest access was rolled out for Microsoft Teams you went nuts and invited everyone you knew and were invited by everyone you knew into a guest team.  It was fun to play around with but the unfortunate consequence was that you now had a bunch of guest access tenants in your organization switcher.  I&#8217;ve seen some pretty funny ones with too many to even speak of.

Thankfully the Azure and Identity Team recently released a process by which you can &#8220;<a href="https://docs.microsoft.com/en-us/azure/active-directory/active-directory-b2b-leave-the-organization" target="_blank" rel="noopener">leave an organization</a>&#8221; and doing so will fix and clean up your Teams organization switcher.

So what does it look like?  You might have something like this:

<img class="alignnone size-full wp-image-1521" src="https://masteringlync.com/wp-content/uploads/2018/05/1-2.png?resize=363%2C133&#038;ssl=1" alt="" width="363" height="133" /> 

But when you login to &#8220;Da Who (Guest)&#8221; you see nothing.  Long ago, I thought I could &#8220;remove&#8221; this by simply leaving the Team I was invited too.  But instead, I just got a big blank screen.

<img class="alignnone wp-image-1522" src="https://masteringlync.com/wp-content/uploads/2018/05/2-1.png?resize=600%2C375&#038;ssl=1" alt="" width="600" height="375" /> 

Here you can see I&#8217;m in the &#8220;Da Who&#8221; organization but I left the Team I was invited to and now there is absolutely nothing let.  I can&#8217;t Chat, no Teams to chat with, nothing.  It&#8217;s just cluttering up my switcher.

So head over to: https://myapps.microsoft.com/

Once you are logged in you should see something like this:

<img class="alignnone size-full wp-image-1524" src="https://masteringlync.com/wp-content/uploads/2018/05/3-1.png?resize=337%2C563&#038;ssl=1" alt="" width="337" height="563" /> 

Click on the Gear Box and you will see all of the organizations you have access to.  You might need to &#8220;Sign In&#8221; so you can leave.  For example:

<img class="alignnone wp-image-1525" src="https://masteringlync.com/wp-content/uploads/2018/05/4-1.png?resize=600%2C316&#038;ssl=1" alt="" width="600" height="316" /> 

And once signed in it will change to Leave Organization.

<img class="alignnone wp-image-1526" src="https://masteringlync.com/wp-content/uploads/2018/05/5-2.png?resize=600%2C331&#038;ssl=1" alt="" width="600" height="331" /> 

After that, the switcher will be all cleaned up for you.

<img class="alignnone size-full wp-image-1527" src="https://masteringlync.com/wp-content/uploads/2018/05/6-1.png?resize=348%2C550&#038;ssl=1" alt="" width="348" height="550" /> 

And after about 15 minutes, go ahead and log out and exit Microsoft Teams.  It&#8217;s critical that you exit the application.  But when you log back in you will see the organization switcher no longer has that organization anymore.

&nbsp;

&nbsp;