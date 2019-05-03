---
id: 1411
title: Making Skype for Business + Microsoft Teams Play Nice Together
date: 2017-10-16T01:45:26+00:00
author: Richard Brynteson
excerpt: "Skype for Business and Teams don't have to compete against each other.  Review how to make Skype for Business and Teams work nicely together in your enterprise environment."
layout: post
guid: http://masteringlync.com/?p=1411
permalink: /2017/10/16/skype-for-business-microsoft-teams/
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
colormag_page_layout:
  - default_layout
image: https://masteringlync.com/wp-content/uploads/sites/2/2017/10/TeamsEatingSfB.png
categories:
  - Microsoft Teams
---
Ignite 2017 is over and there was a lot of talk going into the event that Skype for Business was dead and Microsoft Teams was the future.  There are obvious some aspects of this statement that are very true.  Microsoft made very clear at Ignite that Teams was going to see significant investment in resources but they also made it very clear that Skype for Business Server 2019 was going to see a significant update for on-premise voice users.

So this begs the question, how do we make the two products live side-by-side in an effective manner?  Microsoft Teams has a place in nearly all organizations.  It does team collaboration very well but if you have tried to use Teams and Skype for Business at the same time you may have found that you are going slightly crazy.

So below are some setting suggestions that an organization could use to reduce confusion.  If you haven&#8217;t logged into the Office 365 Administrative portal to see what your options are, here is a quick screen shot:

<img class="alignnone wp-image-1412" src="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2017/10/01.png?resize=500%2C640&#038;ssl=1" alt="" width="500" height="640" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2017/10/01.png?w=1058&ssl=1 1058w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2017/10/01.png?resize=234%2C300&ssl=1 234w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2017/10/01.png?resize=768%2C984&ssl=1 768w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2017/10/01.png?resize=800%2C1024&ssl=1 800w" sizes="(max-width: 500px) 100vw, 500px" data-recalc-dims="1" /> 

<img class="alignright wp-image-1413 size-medium" src="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2017/10/02.png?resize=167%2C300&#038;ssl=1" alt="" width="167" height="300" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2017/10/02.png?resize=167%2C300&ssl=1 167w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2017/10/02.png?w=295&ssl=1 295w" sizes="(max-width: 167px) 100vw, 167px" data-recalc-dims="1" /> 

<span style="text-decoration: underline">Suggestion #1</span> &#8211; Turn off &#8220;**Allow Users to Chat Privately**&#8220;.  When this setting is changed from On to Off the &#8220;Chats&#8221; menu disappears from the Microsoft Teams client.  You can see this in the image to the right that the Chat option is missing from the left menu bar now.

What this accomplishes for you as an organization is that it force all Instant Messages back to the Skype for Business client.  This has an immediate advantage of reducing one area of notifications that could cause confusion within your organization.

In the last 6 months the number of times I&#8217;ve heard someone tell another employee that they IM&#8217;ed them and never heard back to find out that the users was looking in Skype for Business but the message was actually in Teams.

&nbsp;

<span style="text-decoration: underline">Suggestion #2</span> &#8211; Turn off &#8220;**Allow private calling**&#8220;.  This one makes a bunch of sense if you have an Enterprise Voice (EV) deployment.  In nearly all EV deployments I&#8217;ve done there has been a deployment of IP Phone to some set of users (or to all users).  When you allow this setting to remain on, then it is very possible (and it will happen) that users will be on a Skype for Business call and receive a second call in Teams.

For users who have an IP Phone this introduces the real possibility of having two calls running at the same time.  Maybe for those in the IT Pro work space we could somehow manage this array of communication possibilities but for normal users this will result in needless help-desk calls.

For those who are using the soft client, I believe the situation is even worse as the Teams client and the Skype for Business client will fight over the headset.

&nbsp;

<span style="text-decoration: underline">Suggestion #3</span> &#8211; Turn off &#8220;**Allow scheduling for private meetings**&#8220;.  This one might be the most controversial suggestions but I think it&#8217;s an absolute must.  If your intent is to use Microsoft Teams as collaborative tool for specific teams then you should not be allowing team members to create meetings outside the channel.

What this does is when scheduling a meeting, it removes the option to have None under the channel option.

<img class="alignnone wp-image-1414" src="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2017/10/03.png?resize=500%2C89&#038;ssl=1" alt="" width="500" height="89" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2017/10/03.png?w=1800&ssl=1 1800w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2017/10/03.png?resize=300%2C53&ssl=1 300w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2017/10/03.png?resize=768%2C136&ssl=1 768w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2017/10/03.png?resize=1024%2C181&ssl=1 1024w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2017/10/03.png?w=1600&ssl=1 1600w" sizes="(max-width: 500px) 100vw, 500px" data-recalc-dims="1" /> 

This essentially returns Teams to it&#8217;s GA state for meeting scheduling but this will prevent Team members from scheduling events outside their channel and reducing friction for the users.  I would argue that preventing this &#8220;out of channel&#8221; scheduling is actually beneficial to the team as meetings that were meant to be recorded/available to all are often times scheduled outside the channel losing all of the playback options.

&nbsp;

Hopefully you find some of these suggestions helpful.  Hopefully as time goes on Microsoft allows us to be more granular in the above settings but this creates a meetings first approach to your transition to Teams.

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;