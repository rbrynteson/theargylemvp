---
id: 1345
title: 'Quick Tip: Microsoft Teams Under The Hood'
date: 2016-11-07T20:10:17+00:00
author: Richard Brynteson
layout: post
guid: http://masteringlync.com/?p=1345
permalink: /2016/11/07/quick-tip-microsoft-teams-under-the-hood/
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
  - Microsoft Teams
---
In this post we are going to look at some of the under the hood of Microsoft Teams.  During the keynote it was mentioned over and over again it was &#8220;Skype&#8221;.  But what does that mean?  You might ask, this is a business product so it must be built on top of Skype for Business but that isn&#8217;t the case at all.

So let&#8217;s look at each modality.

**Instant Messaging**

If we send a message we see two different JSON events.  First, we see a typing command to update the team so they know I&#8217;m in the processing of replying.

<img class="alignnone size-full wp-image-1347" src="https://masteringlync.com/wp-content/uploads/2016/11/2-3.png?resize=212%2C76&#038;ssl=1" alt="2" width="212" height="76" data-recalc-dims="1" />

Then when the message is actually sent we see this JSON message.

<img class="alignnone wp-image-1346 size-full" src="https://masteringlync.com/wp-content/uploads/2016/11/1-3.png?resize=300%2C117&ssl=1 300w" sizes="(max-width: 325px) 100vw, 325px" data-recalc-dims="1" />

So there are a few items of note that we should discuss at this time:

  * This is absolutely not Skype for Business under the hood.  Microsoft has talked about the next gen Skype Consumer network and this is clearly go to that environment.https://st-client-ss.msg.skype.com/v1/users/ME/conversations/19%3A2a9ad786-8a9b-409c-a29a-19666d549187_fabf899c-a61b-44a3-9623-d269342541b4%40unq.gbl.spaces/messages
  * This is all driven via JSON and web services.  If you look at the language that is being used this is a very modern interface that is be used by several other players in the market.

**Audio and Video Sessions**

When you join an Audio/Video session we can see a very similar JSON setup experience.  First, here is sent to the Teams Server.  Some interesting URLs.

<img class="alignnone wp-image-1349" src="https://masteringlync.com/wp-content/uploads/2016/11/3-3.png?resize=600%2C194&#038;ssl=1" alt="3" width="600" height="194" data-recalc-dims="1" />

And here is the response:

<img class="alignnone wp-image-1353 size-full" src="https://masteringlync.com/wp-content/uploads/2016/11/1-4.png?resize=456%2C227&#038;ssl=1" alt="1" width="456" height="227" data-recalc-dims="1" />

Again, we see the full URL here: https://api.conv.skype.com/conv/

So again, we see the entire meeting experience is built on top of the Skype Consumer network.  So the key item to know is that Teams sits on that next generation Skype Consumer network and is most likely one step to a much larger consolidation.