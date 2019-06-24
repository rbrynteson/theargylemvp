---
title: Migrate from WordPress to GitHub Pages
date: 2019-06-24T14:37:39+00:00
author: Richard Brynteson
layout: post
permalink: /2019/06/24/why-not-move/
categories:
  - Microsoft Teams
  - Skype for Business
---

Migrating to Microsoft Teams can be an easy win for many organizations.  Those who are using Skype for Business or another technology for presence and messaging should take advantage of the amazing opportunities of Microsoft Teams.  Collaboration with team members can greatly reduce time to market of products, increase productivity and change the company culture.

However, if you are currently using Skype for Business on-premises (and online to a lesser degree) there might be lots of reasons why an upgrade might not be the right solution for you.

## The Migration

The biggest thing to remember is that moving from SfB to Teams isn't an upgrade as Microsoft likes to sell it.  Teams is a migration from one platform to a completely different.  This creates new challenges for organizations that you may have never thought of.

Recently I posed the question to Twitter as I received an internal Microsoft survey for to list the reasons customers are not moving to Teams.  This is one of those instances where the internet did not let me down.  I've tried to categorize all of the items as best as I could.  Some items Microsoft can't really "solve" but most others they certain can.  I've tried to put them into semi-logical groups so I could go in-depth on each group.  Some items cross groups and where possible I've tried to suggestion solutions for Microsoft for this.

### Group 1

- Lack of trust in cloud solutions
- Existing on-premises infrastructure costs and desire to not repurchase
- Prefer cap-ex model vs op-ex model
- Time.  Just finished SfB Migration

This is the hardest group for Microsoft to handle.  Customers who pick these items are more than likely adverse to the cloud or it simply doesn't make sense.  For example, I know LOTS of organizations that either just finished or are still in their migration to SfB from a legacy phone system.  To ask those customers to make another telephony change this fast is asking way too much.

At CommsvNext I recall hearing a MS employee say that they believe phone systems are replaced every 3 to 5 years.  I also did a spit-take across the table.  Organizations are coming from a world of 20 to 30 years with a PBX.  SfB was an adjustment because it was software that we continued to updated with new features.  To say an organization is on a 3 to 5 year cycle for their PBX is pretty funny.

### Group 2

- Still needs on-premises sbc/hardware for analog support
- Poor analog support
- Lacks paging integrations
- Lack of survivability
- Teams client lacks survivability

In this group I see the general trend to be hardware related.  For example, if I'm an organization that requires paging this becomes a serious issue.  Of the 40+ SfB deployments I see on a regular basis, I would guess that 30 to 35 of them have some sort of device to accomplish paging.  This could everything from an old SNOM devices or other native endpoints or devices that SIP register to their SBC.  Regardless, in both of these cases, Teams doesn't provide a like-for-like model or requires the existing SBC to remain on premises which devalues the Teams Voice move.

The biggest item in this group is survivability.  Teams has suffered a handful of timely voice outages and the client doesn't survive a single outage well.  Some of this could be offset if the Teams client had some sort of survivability against an on-premises SBC used for Direct Routing.  If this was possible, it would be a massive win and provide all sorts of possibilities into the future.

### Group 3

- 3rd Party Software not updated (Lacks API's)
- No Recording API
- No VDI Support
- Contact Center
- Lack of E911
- Federation Support

This group I think contains two items of note.  First is extensibility of the Teams product.  Given UCMA doesn't exist, 3rd Party integrations are dependent on what Microsoft exposes in the Graph API.  And with no client side SDK, it really limits what is possible.  So many solutions where built on this level of extensibility that simply doesn't exist in today's product.  Obviously we know some items are coming via the Public Roadmap but so much is unknown.

The last is federation.  And this I think goes into two sub-buckets.  Native federation which we know Microsoft has said is coming.  The second revolves around the absolutely confusing aspect of Guest access and how you communicate with those users.  Currently, I can count four different ways you can add/talk to someone in Teams via Federation V1, Guest access in Teams, meetings and so much more.  This story is confusing to users.  Obviously, its easier if Skype for Business didn't exist but even with Teams alone the story is confusing.

### Group 4

- PSTN Conference Costs (only have E3)
- Lack of Conference Direct Routing Option (i.e. they have massive conference requirements and purchasing E5 or license is prohibitive)
- Pay by the drink for minutes
- Missing integration with legacy telco products (SIP)
- Missing calling plans in desired geo
- Legacy rooms can't join partner rooms
- Room Migrations/Devices lacking.  Already purchased existing Polycom Trio for example.
- Common Area Devices are too costly

Now we start to get into what I consider an odd grouping of items.  I'll try to explain the general category as integration/conference rooms.  For years Microsoft pushed different processes and methods by which to get legacy equipment into a meeting.  This was done with Direct SIP, VIS Server and much more.  The story is not simpler but can be expensive.  Getting third party products into rooms requires the purchase of more equipment and more licenses.  For an organization that has already spent a good deal of cap-ex on this hardware to now have to spend more can be a tough ask.  Especially when competing products do this integration for free.

Also in this group is what i would consider costs/option issues.  I hear from customers often that the cost for a Common Area phone is simply too much!  For large enterprise organizations, the common area phone isn't a second class citizen but is a first class citizen.  It's not unusual to see one common area device for every 4 to 5 users.  So for a company of 80,000 - yes it's very possible to see nearly 20k common area phones.  In SfB, these devices came along with no license needed.  There is of course missing geo issues and more.

### Group 5

- User confusion of product
- Too many changes in Teams
- Contact Search
- Native IP Desk Phones are too expensive
- Native IP Desk Phones lack features
- Existing LPE phones
- General user dislike of the client
- Lack of monitoring tools
- Lack of troubleshooting tools
- Too much downtime of Teams
- Expectation of 3 replacement cycle of devices is unrealistic
- Moving Target (features, options, modes)

Honestly, we are into a catch all now.  The biggest grip I hear from customers is around devices.  Either they have already purchased phones and don't want to buy them again or the expectation that IP Phones are now a commodity product doesn't sit well with customers.  If I need to purchase 20,000 devices and they have a shelf-life of three years, I'm not going to buy that product.  This could easily be taken care of if Microsoft posted a guideline or supportability guidelines around their phones.  Microsoft Teams doesn't have a single reference in the Microsoft Product life-cycle website.  That is weird and for a corporate customer that can be a big deal.

Lastly, there is the pace of Teams.  I hear this often.  It's great that features are coming all the time but they always launch with them enabled.  We talk about Shadow IT and I understand if a feature is disabled it might take a long time for an organization to adopt it.  But I think there is a notion of Change Fatigue that users suffer from as well.  The ring method is already confusing but maybe Microsoft creates a Ring 5 where features are delayed 1 to 2 months from general release.

### Group 6

- Business Premium licensing missing phone system

OK, this one could be an easy win.  And honestly, I've never understood why as a Business Premium users can't use Phone Calling.  Of all of the markets that Microsoft could easily take advantage of it's here.  Why not compete with Ring Central and others in the less than 300 user market.

Hopefully these make sense and of course feel free to ask questions below.  Teams is amazing - I love it - but SfB isn't going to go away for a lot of organizations due to the handful of items in this list.
