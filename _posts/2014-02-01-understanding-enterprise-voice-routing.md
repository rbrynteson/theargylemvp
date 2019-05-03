---
id: 613
title: Best Practice when Deploying Enterprise Voice
date: 2014-02-01T03:16:56+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=613
permalink: /2014/02/01/understanding-enterprise-voice-routing/
st_layout_box:
  - right
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
Deploying Enterprise Voice can be a complicated task but following a few simple suggestions can make the experience easier for your organization.

**Format Phones Numbers Properly**

There are two major categories: Active Directory and within Lync.

_Active Directory_

When storing numbers in Active Directory, you should save every number in E.164 format.  E.164 is an internationally recognized standard for phone numbers.

+<CountryCode><City/AreaCode><LocalNumber>;ext=<ext>

The country code is 1 to 3 digits long.  Most countries do not share a code but there are some cases like country code 1, which is shared between the United States, Canada a other locations.  The city, area code and local number will all vary between six to fourteen numbers in length.  The ext= is used as the extension.

NOTE: Lync supports a maximum extension length of eight digits.

A phone number formatted in E.164 would look like:

+18003233639

When putting phone numbers in Active Directory, you can add separators for readability.  For example, you may want to format a number as +1 (800) 323-3639.

When dealing with internal extensions, your number will require the ;ext= at the end of the number.  You will need to select a common number to be used as the root of your internal number.  This number could be your billing telephone number.  A good option would be to use the number that is associated with your company directory.  An internal extension would look like:

+19528310888;ext=0888

Never store the telephone number as just the extension.  For example, putting just 0888 in Active Directory is always bad.

Lastly, never add country specific routing codes for international dialing within Active Directory, like 011 or 00.  These routing codes are not part of the E.164 standard but rather escape codes for international calling.

If your organization cannot modify your telephone numbers into proper E.164 within Active Directory, you can use the company\_Phone\_Number\_Normalization\_Rules.txt file on the server to modify your Active Directory numbers to E.164.  However, there can be limitations, and you need to ensure all of your numbers are consistent within Active Directory.

_Phone Numbers within Lync_

Like your phone numbers within Active Directory, your phone numbers (or LineURI&#8217;s) within Lync will also be in E.164 format.  The difference is, there is no option to not have them in this format.  So your LineURI should be formatted as:

+<CountryCode><City/AreaCode><LocalNumber>;ext=<ext>

One common mistake is organizations will leave the extension field off because they believe they have no non direct inward dial (DID) numbers.  Although this format would work:

+19528310888

Leaving the ;ext= off of the phone number can cause some unexpected behaviors during your deployment.

**Minimize Dial-Plans**

When you are in process of migrating from your legacy PBX to Microsoft Lync its impossible to avoid having multiple dial-plans within the organization.  You will go from one device to manage your dial-plan to three different systems which could host a dial-plan: Lync, SBC and legacy PBX.  The goal when migrating from your PBX to Lync is to minimize the amount of work that needs to occur within the legacy PBX.  It doesn&#8217;t make any sense if you have to make wholesale changes to an environment you are about to remove from your enterprise.

In order to minimize these changes,  you should follow these rules.

1. Keep the rules on your gateway/SBC as simple as you can.  Lync Server 2013 allows you to modify both the called and calling numbers so there is little reason for overly complex rules on the gateway.  Additionally, the gateway is often times the most complicated device within the environment and you should minimize changes whenever possible.  Your Sonux UX1000/2000 has Active Directory integration which allows you to move users to Lync without having to make any gateway changes.  Always take advantage of this functionality.

2. Send the legacy PBX exactly what it needs.  Don&#8217;t mess with routing or adaption tables within the PBX if you do not have to.  You can easily use called number rule to strip down a number like +17635551234 to only 1234 to send to your PBX.

Hopefully this these best practices will help you with your Enterprise Voice Deployment.

&nbsp;

&nbsp;