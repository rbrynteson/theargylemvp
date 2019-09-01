---
id: 1498
title: Guest Access, Microsoft Teams and the Mobile Experience
date: 2018-03-29T15:03:29+00:00
author: Richard Brynteson
layout: post
guid: http://masteringlync.com/?p=1498
permalink: /2018/03/29/guest-access-microsoft-teams-and-the-mobile-experience/
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
So there have been A LOT of talk about Guest Access and Microsoft Teams so I figured it is time to take a bit of a deeper look at what is and isn&#8217;t available at this time.  I&#8217;m going to highlight a few great additions and then also some items that just are not quite there yet.

**Guest Access to Office 365 Accounts**

So this is the oldest and most know version of guest access today.  You have someone else (Commercial or Education) who uses Office 365 and you want to add them as a Guest to your Team.  This was announced and explained back in a September 11, 2017 <a href="https://blogs.office.com/en-us/2017/09/11/expand-your-collaboration-with-guest-access-in-microsoft-teams/" target="_blank" rel="noopener">Blog Post by Microsoft</a>.  What is happening under the hood is that user from Tenant G (for Guest) is being provided Azure AD access to your Tenant A.  We can see this in both the Office 365 Portal and the Azure AD Portal.

<img class="wp-image-1500 size-large" src="https://masteringlync.com/wp-content/uploads/2018/03/2-1.png?resize=800%2C465&#038;ssl=1" alt="" width="800" height="465" /> 

Azure AD Portal<img class="wp-image-1499 size-large" src="https://masteringlync.com/wp-content/uploads/2018/03/1-1.png?resize=800%2C459&#038;ssl=1" alt="" width="800" height="459" /><figcaption class="wp-caption-text">Office 365 Portal</figcaption></figure> 

So this was a big step but not all is solved yet in this realm.  There are still a few areas you need to be aware of when dealing with an Office 365 Guest Account.  <span style="text-decoration: underline">In particular, what if Tenant G has Microsoft Teams disabled</span>?  As of about two months ago we got a solution at the browser level.  If a user from Tenant G logs into Microsoft Teams they will now see this page:<figure id="attachment_1501" style="width: 800px" class="wp-caption alignnone">

<img class="wp-image-1501 size-large" src="https://masteringlync.com/wp-content/uploads/2018/03/3-1.png?resize=800%2C467&#038;ssl=1" alt="" width="800" height="467" /> <figcaption class="wp-caption-text">Guest Access Picker</figcaption></figure> 

In the screen shot above, you will see where it says &#8220;Office (Guest)&#8221;.  This is the name of Tenant A as defined in Office 365.  Well, the &#8220;Office&#8221; part is.  So if this user was Tenant G was invited to 10 different guest tenants, although Teams is disabled for Tenant G, that user can still get to the desired location via the picker.  This is a nice addition.

Unfortunately, we are starting to see a larger product gap between Desktop/Web and the Mobile Clients and it feels like it&#8217;s growing weekly.  Although they share a significant code base there is still some structural barriers.  If the user from Tenant G attempts to login to their iOS or Android Client (we won&#8217;t talk Windows Phone as that client is very behind now) the user will receive a message saying they cannot login as Teams is disabled for their tenant.  To make matters worse, if the user attempts to login via the browser on their phone they are redirected to the Store or App on the phone.

**Guest Access for MSA Accounts**

At the end of February, <a href="https://techcommunity.microsoft.com/t5/Microsoft-Teams-Blog/Collaborate-securely-with-anyone-in-Microsoft-Teams/ba-p/165941" target="_blank" rel="noopener">Microsoft announced</a> guest access was coming to Microsoft Teams for &#8220;anybody&#8221;.  And although this is true its important that we put a small &#8220;asterisk&#8221; next to the anybody comment.  When you invite a new user to Teams and they go through the registration process their is a MSA (MicroSoft Account) requirement.  Although the Microsoft Teams did a fantastic job of making this process as easy as possible we can see it&#8217;s still required.

<img class="alignnone wp-image-1502 size-large" src="https://masteringlync.com/wp-content/uploads/2018/03/4-1.png?resize=800%2C465&#038;ssl=1" alt="" width="800" height="465"  /> 

When you click the &#8220;Next&#8221; button, there is a check that occurs to see if the e-mail address invited is an MSA Account or not.  If it is not, then you are walked through the process to &#8220;Create Account&#8221;.  Although you might be thinking you are creating a &#8220;Teams Account&#8221; under the hood it&#8217;s a MSA Account.  I don&#8217;t think this is underhanded or anything but is important to know, it&#8217;s not &#8220;True&#8221; Guest access as there is still a requirement for MSA.

<img class="alignnone wp-image-1503 size-large" src="https://masteringlync.com/wp-content/uploads/sites/2/2018/03/5-1.png?resize=800%2C464&#038;ssl=1" alt="" width="800" height="464"  /> 

Once you have gone through and created your account all is good.  You have access to your team and it just works.  <span style="text-decoration: underline">But what about mobile? </span> Again, we see the disconnect between the Desktop/Web verses the Mobile client again.  Although I can login to Microsoft Teams as my MSA Account (which is most likely why you need the MSA account &#8211; it makes sense to me) you can&#8217;t do anything with it.

<img class="alignnone wp-image-1504 size-medium" src="https://masteringlync.com/wp-content/uploads/sites/2/2018/03/Screenshot_20180329-155743.png?resize=154%2C300&#038;ssl=1" alt="" width="154" height="300" /> 

You are presented with the ability to &#8220;select an account&#8221; but nothing happens.  Says the feature is &#8220;coming soon&#8221;.  So again, the lag of the mobile client hampers the Teams Guest experience.

&nbsp;

Don&#8217;t get me wrong, I believe the Teams and Guest access are fantastic steps forward.  And although I didn&#8217;t get into the &#8220;Guest Switching&#8221; behavior which I think we can all agree needs some work, the steps the Teams team has done in only 13 months is absolutely fantastic.  I&#8217;m looking forward to what is available in the next 13 months as well.

&nbsp;

&nbsp;

&nbsp;