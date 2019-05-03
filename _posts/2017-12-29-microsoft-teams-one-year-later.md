---
id: 1470
title: 'Microsoft Teams &#8211; One Year Later'
date: 2017-12-29T18:44:02+00:00
author: Richard Brynteson
layout: post
guid: http://masteringlync.com/?p=1470
permalink: /2017/12/29/microsoft-teams-one-year-later/
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
It was just over one year ago that Microsoft Teams hit the streets in a public preview form and later went to General Availability.  At the time, I [wrote](http://masteringlync.com/2016/11/02/microsoft-teams-a-not-so-brief-review/) extensively about a few of the highlights and low lights of the Microsoft Teams.  Overall I said it was a game changer and I still believe that is today.  Over the past year we have seen the direction of Microsoft go from another Social Chat application (competitor to Slack) to a full fledged replacement to Skype for Business and the traditional PBX.  So let&#8217;s first talk about some of the changes.

**December 2017 Check-Point**

Microsoft released a fairly large roadmap for Teams right after Microsoft Ignite that was focused primarily on Skype for Business and Teams interop.  But the roadmap was only looking at how to connect Skype for Business and Teams together but we knew that some other changes had to be happening behind the scenes and we have not been let down.  Some of the notable updates have been:

**Teams &#8211; Edge and Chrome Browser Support for Meetings. ** This IS HUGE!  If you have used Skype for Business you are painfully familiar with the download the SkypeWebApp plug, refresh the browser, hope it connects to the meeting and when it does go through the setup of devices, etc.  It&#8217;s not the greatest experience.  Skype for Business CU5 introduced some changes including the option to load the plugin from Azure instead of local, a refreshed GUI but it still requires a plugin.  As browsers move away from the old plugin model its just a matter of time until something breaks (see Firefox 52 and later as an example).

So now with Edge and Chrome (and have to imagine Firefox in time &#8211; who knows about the closed garden of Safari) can join a Microsoft Teams meeting directly from the browser.  And not just chat, but Audio and Video without the need of a plug-in.  This is a game changer for Microsoft.  It brings modern meeting and modern browsing together.  We learned at Ignite this is never coming to Skype for Business so a big update for Teams.

UPDATE 12/29/2017: So I tested this with a Guest User on a Chromebook and the guest was able to join with Audio but no Video at this time.  Not sure if it&#8217;s all chrome users have no video or just guests but more testing needs to happen.

**Teams &#8211; Application Sharing.**  Previously, you should share your desktop.  And that was it.

<img class="alignnone wp-image-1471" src="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2017/12/Untitled.png?resize=600%2C219&#038;ssl=1" alt="" width="600" height="219" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2017/12/Untitled.png?w=840&ssl=1 840w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2017/12/Untitled.png?resize=300%2C109&ssl=1 300w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2017/12/Untitled.png?resize=768%2C280&ssl=1 768w" sizes="(max-width: 600px) 100vw, 600px" data-recalc-dims="1" /> 

Although this is a welcome change and addition to Teams they still have no addressed my #1 complaint about sharing.  The mini-window!!!

<img class="alignnone size-full wp-image-1472" src="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2017/12/v2.png?resize=279%2C160&#038;ssl=1" alt="" width="279" height="160" data-recalc-dims="1" /> 

When you share the Desktop or an Application this is what you get for Microsoft Teams.  What if I wanted to share my desktop and participate in a Chat with someone?  Nope!  In the future they have promised Lobby control for meetings.  Will I have those if I&#8217;m in a desktop/application share?  Hard to say.  This is something that needs to change and hopefully it changes sooner than later.

**PSTN Calling!**  So a lot has been made of CloudPBX and the switch to Microsoft Phone System and where Skype for Business On-Prem fits into the future (spoiler alert &#8211; on-prem SfB will not go away in the next 5+ years &#8230; maybe 10+ years).  However, if you are an online only user and need PSTN access then Microsoft Teams might be the right fit for you.  In the past 60 days we have seen Call Blocking, Blind Transfer, e911 Support, Caller ID Masking, Extension Dialing, Hold, Multi-Call Handling, Sim Ring &#8230; and the list goes on!  Microsoft said at Ignite they were going to have an aggressive Microsoft Teams build out and they have met most of their deadlines.

**Big Teams! ** At the start Teams was limited to just under 250 users, today that number stands at 2500!  So for many small-mid sized companies who used Yammer we have seen a quick shift to Teams as it simply provides a larger array of options for large groups.  And which recent options to control who can post in General I believe you will see the shift happen ever faster.  For the 2500+ organizations, Yammer isn&#8217;t impacted too much &#8211; but I&#8217;m sure bigger teams is coming in 2018.

I won&#8217;t go through every new feature that made its way to Microsoft Teams as there are a lot of good resources for that.  <a href="https://blog.thoughtstuff.co.uk/2017/12/microsoft-just-launched-27-new-features-for-teams/" target="_blank" rel="noopener">#1</a>, <a href="https://lucavitali.wordpress.com/2017/10/01/sfb-teams-features-comparison-table/" target="_blank" rel="noopener">#2</a>, <a href="https://support.office.com/en-US/article/Release-notes-for-Microsoft-Teams-d7092a6d-c896-424c-b362-a472d5f105de" target="_blank" rel="noopener">#3</a>  This has been a quick list of what I think are the most notable improvements thus far.

&nbsp;

**What is Next?**

So what areas need to happen sooner than later for the migration of Skype for Business to Teams or simply Teams over Slack to continue.  I think it boils down to a few areas.

<span style="text-decoration: underline">Guest Access:</span>  There has been numerous articles about the state of guest access with Microsoft Teams but I will bring it down to just a few major changes I think need to happen in 2018.

#1 &#8211; Faster tenant switching!  I&#8217;m guessing that today, the vast majority of Teams users are in a single organization and it&#8217;s not a problem.  But as Teams use continues to grow and for service companies who continue to drive collaborative chat, the number of organizations you belong too is simply going to increase.  Today, guest switching is Yammer switching.  No other way to describe it.  You choose which tenant you want to login to, you wait 30 to 90 seconds, the app goes away, comes back and suddenly you are there.  Unfortunately, you are completely logged out of your old tenant so simple things like, taking a telephone call, replying to an chat, etc. are no longer possible.  Microsoft should adopt a framework like Slack where you can be logged into multiple Tenants at the same time from one window.  Or allow multiple instances of Teams logged into different tenants and utilize something like &#8220;Sets&#8221; in Windows 10.  Regardless, it&#8217;s broken and needs to be cleaned up.

#2 &#8211; More options.  This is on the roadmap but isn&#8217;t happening fast enough unfortunately.  I would argue that true guest access &#8211; i.e. any e-mail address on any system &#8211; is a bigger priority than many of the roadmap items today.  I understand the security and compliance issues behind it, it&#8217;s a huge task but one we all know needs to be resolved.

#3 &#8211; Federation.  Nothing more needs to be said.

<span style="text-decoration: underline">Meetings:</span> This is another area of investment that I think could be improved upon for 2018.  The roadmap and the demos at Ignite looked amazing but a few items should happen sooner than later.

#1 &#8211; Mini-Window.  I mentioned it above, desktop/app share results in an unusable Teams experience.  We need options here.

#2 &#8211; Recording.  We know it&#8217;s coming.  It&#8217;s on the roadmap its just too far away.

<span style="text-decoration: underline">UI Changes</span>:  This is different than my desktop/app share mini-window comment but there needs to be an option for a smaller window size with fewer clicks.  Think about it for a second, if you are in a Teams chat and want to chat with someone individuals, it&#8217;s at least two clicks away and then back, etc.  Teams does an amazing job of being a collaborative tool but it doesn&#8217;t do a great job of being a real-time communications tool.  There are a lot of options that could be done to address this.  Unfortunately, most will require a bit of a rethink on the UI design of the product.

&nbsp;

Don&#8217;t get me wrong.  Microsoft Teams is here to stay and it&#8217;s an amazing product.  I just wish there were a few changes to the tool to make it even better!

&nbsp;