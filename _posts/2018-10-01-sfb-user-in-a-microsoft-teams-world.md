---
id: 1677
title: SfB User in a Microsoft Teams World
date: 2018-10-01T08:30:30+00:00
author: Richard Brynteson
layout: post
guid: http://masteringlync.com/?p=1677
permalink: /2018/10/01/sfb-user-in-a-microsoft-teams-world/
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
  - Skype for Business
---
So with Microsoft announcing full interop between Skype for Business On-Premises and Microsoft Teams I decided it was time to take the dive and make some changes to our Office 365 tenant.  So what did that journey look like.  Let&#8217;s start at the beginning:

**Day 1**

Setup hybrid for Skype for Business.  This isn&#8217;t very hard in SfB 2015 and there is a wizard from within the SfB Control Panel to accomplish these tasks.  So the tasks were:

#1 &#8211; Make sure that Azure AD Connect was functioning like we thought it should be.

#2 &#8211; Run the SfB Hybrid wizard and ensure that Get-CsTenantFederationConfiguration -SharedSipAddressSpace is set to $True.

This setting can take up to 24 hours to apply.  So off to do some other work.

**Day 2**

Migrate the user you want to be Teams only from your Skype for Business On-Premises to SfBO.

Move-CsUser -Identity sip:user@domain.com -Target sipfed.online.lync.com -Credential $cred -HostedMigrationOverrideUrl https://admin1a.online.lync.com/HostedMigration/hostedmigrationservice.svc

NOTE: Remember to update/change admin1a to your correct SfBO tenant.

**Day 3**

Next we need to configure our environment into the right mode.  By default, your Teams Upgrade policy is either in Legacy or Islands.  Both modes are eventually going to cause all sorts of heartache and issues for you.  I would HIGHLY recommend getting out of either of these modes as fast as possible.

Since our organization is a Skype for Business shop it was easy enough to set our organization default to SfB Only.

<img class="alignnone wp-image-1678 size-full" src="https://masteringlync.com/wp-content/uploads/2018/10/Modes.png?resize=800%2C280&#038;ssl=1" alt="" width="800" height="280" /> 

As you can see, there are three modes currently supported.  In the near future, we will see two additional modes related to Skype for Business interop and those are Skype for Business + Teams Collab and Skype for Business + Teams Collab + Meetings.  I think Microsoft announced at Ignite we should see those by early 2019 if my memory is correct.

The second item we need to do is setup our Teams to allow it to Chat/Interop with Skype for Business. By default, this is disabled.  So from the Modern Portal go to Teams Settings under Org Wide Settings.  Scroll down a bit and you should see this:

<img class="alignnone size-full wp-image-1679" src="https://masteringlync.com/wp-content/uploads/2018/10/Interop.png?resize=800%2C146&#038;ssl=1" alt="" width="800" height="146" /> 

Next, we need to setup a Teams Only user because that is the whole point of this exercise.  So again, from the Modern Portal, you will go to Users and find the user you want to move to MS Teams:

<img class="alignnone size-full wp-image-1680" src="https://masteringlync.com/wp-content/uploads/2018/10/TeamsOnly.png?resize=364%2C332&#038;ssl=1" alt="" width="364" height="332" /> 

Now this is the important part!!!  For this interop to work correctly, the user who you set to Teams Only **MUST HAVE BEEN** migrated to Skype for Business Online.  If they are a Skype for Business On-Premises user nothing is going to happen here.

These routing changes can take up to 24 hours to complete.  So it&#8217;s time to wait again.

**Day 4**

Everything should work.  When people IM or Call from Microsoft Teams to Skype for Business On-Premises users it should just work.  It&#8217;s magic!

<celebration time!!!>

&nbsp;

&nbsp;

**It&#8217;s Not Magic!**

OK&#8230;so what if it&#8217;s not magic and doesn&#8217;t work.  Like mine.  A few things to check.

#1 &#8211; Teams to Skype for Business Chat.

This one was a bit confusing at first.  Everyone in our organization was in Islands mode and that means some users had been using Teams, some not so when I tried to Chat/IM from Teams->SfB I found that some users got message in Teams and other in SfB.  It was ALL OVER the place.

The issue was, I was using an existing Teams Chat thread and I thought it would be smart enough to route to the right place.

<img class="alignnone size-full wp-image-1681" src="https://masteringlync.com/wp-content/uploads/2018/10/No.png?resize=401%2C180&#038;ssl=1" alt="" width="401" height="180" /> 

So as you can see above I have two different chats going with Matt.  One in Teams and one in SfB.  I can tell based on the icon.  This because a Skype for Business Only mode doesn&#8217;t actually take away anything from the Teams client.  In the future, this will be changed and my guess is even with that change, if I tried Chat/IM with an existing thread it would go no-where and just cause confusion.

So the key for the Teams user, is to start a new thread using the New Chat button at the top of the page, that will let you create a new Teams -> SfB Thread and then it will show up in Recent and just work.

&nbsp;

#2 &#8211; It&#8217;s really broken.

This is going to happen to what I hope is a very SMALL handful of customers but in case it does here is what happened.  So when I did all of the above, when a Skype for Business user would Chat/IM with me the message it would &#8220;break&#8221; through my upgraded client.  Check out the below:

<img class="alignnone size-full wp-image-1682" src="https://masteringlync.com/wp-content/uploads/2018/10/Updated.png?resize=693%2C540&#038;ssl=1" alt="" width="693" height="540" /> 

So if I was running my SfB Client, it would tell me I was updated and now using Teams but if someone tried to Chat/IM with me it would still appear as a Chat window.  Absolutely crazy stuff.  If I closed/exited SfB on my desktop then the SfB user would get a 480 No Devices Available.

After a lot of looking around, I finally decided to go to MS Support.  As always, the first line of Support was less than great and even tried to close the case telling me this wasn&#8217;t supported.  After getting the right people to look at it, it was escalated and they found the issue real fast.

When MS Support ran the Get-CsTeamsConfiguration -Tenant 06739bd6-&#8230;my&#8230;tenant&#8230;.id&#8230;.52589 | fl * it returned the following:

EnabledForMessaging : False

As you can imagine, not being enabled for Messaging seems odd and it was wrong.  So they have to enable my tenant for messaging and suddenly it all works.  A few notes: #1) This command is not customer facing so you have no clue what it&#8217;s set at.  #2) You have no ability to change that setting either, so who knows how our tenant ended up in this mode.

&nbsp;

I&#8217;ll continue to update this post as I learn new things but as of today I&#8217;m a Skype for Business user living 100% in Teams now.  I&#8217;m guessing federation doesn&#8217;t work, right?  🙂

&nbsp;

&nbsp;

&nbsp;