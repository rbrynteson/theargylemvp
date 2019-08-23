---
title: Where Did My Chat Disappear To?
date: 2019-08-14T15:21:39+00:00
author: Richard Brynteson
layout: post
permalink: /2019/08/14/where-did-my-chat-disappear-to/
categories:
  - Microsoft Teams
---

So you have been migrating users from Skype for Business modes in Teams and decided its time to move a user to Teams Only (or for some horrible reason back to Islands).  But after you migrate your user from a SfB Mode to Teams Only you discover that the user does not have chat in their Teams client.  When you attempt to message the user from another client you see that the Administrator has disabled chat for this user.

<img src="https://theargylemvp.com/assets/images/082301.png" />

Maybe someone messed with policies.  So you go to the user and find the messaging policy set to Global:

<img src="https://theargylemvp.com/assets/images/082302.png" />

And when you dig into Global you find that Chat is enabled.

<img src="https://theargylemvp.com/assets/images/082303.png" />

## Fiddler/Proxy Time

Everything looks right, so how do we check and see what option is actually being given to the client.  You can use a tool like Fiddler or Charles Proxy to find this information.  I  prefer to use Charles Proxy and I suggest you see [Erwin Bierens](https://erwinbierens.com/setup-charles-proxy-for-microsoft-teams/) blog for how to set this up.

Once Charles Proxy is setup you can find "useraggregatesettings" under teams.microsoft.com and if you look at the JSON text you find allowUserChat is set to false. 

<img src="https://theargylemvp.com/assets/images/082304.png" />

Obviously, we have a problem at this point in time.  There are two options:

1. Open PowerShell and connect to SfBOnline and run get-csuseronline user@domain.com | grant-csteamsmessagingpolicy -policyname $null
2. Open a ticket with Microsoft and they can do it.

By running step #1, it forces the policy change to run through the provisioning pipeline again and will update all of the different policy tiers.  This can take up to 24 hours for the change to be visible.  But once it's done, the user will get their chat back.