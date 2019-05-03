---
id: 172
title: Location Based Routing with Lync Server 2013 CU1
date: 2013-02-21T19:07:29+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=172
permalink: /2013/02/21/location-based-routing-with-lync-server-2013-cu1/
st_layout_box:
  - right
categories:
  - Lync Server 2013
---
As announced at the Lync Conference 2013 a new feature, Location Based Routing, will be coming to the product as part of the February 2013 (CU1) update.  This feature will allow administrators to configure enterprise voice routing rules that will control routing based on the physical location of the person.  This is a huge new feature to the product!

Here are the major items to be aware of:

  * Location Based Routing forces outbound calls to be routed to a specific PSTN gateway that the user is physical located at.  This is controlled based on the sites/subnets you have already defined for CAC.  This prevents toll-bypass for these users which in some cases can violate local laws.
  * Location Based Routing also prevents inbound PSTN calls to Lync endpoints if in other location.  Preventing the call to be routed over the WAN.  For example if a user in Minneapolis traveled to India, the inbound call to Minneapolis PSTN Gateway would be prevented from routing over the WAN to India and the call would be sent to Voice Mail instead, assuming it has been enabled for that user.
  * Location Based Routing will prevent PSTN toll bypass on both inbound and outbound calls to and from locations that are not defined or unknown.  So if a site is enabled for location based routing and the user lands on an undefined wireless network (as defined in Lync Server Network/Sites/Subnets) then the call would not be allowed to be routed to that user as the server would not know what site they are currently in.

**Outgoing PSTN Calls**

Calls will be authorized based on the users voice policy.  Therefore in our example of a Minneapolis user visiting the India office, if the user in Minneapolis&#8217; voice policy does not allow the user to call numbers in India, simply because the user has visited the office will not allow them to dial those internationally numbers suddenly.  So it&#8217;s important to remember that the user must be authorized to dial the number based on their voice policy.

**Inbound PSTN Calls**

I just wanted to mention it again, calls will not route over the WAN to the user, is the site is enabled to prevent PSTN Toll Bypass.

**Delegate Users/Sim Ring/Call Forwarding**

It&#8217;s important to think about the implications of these scenarios when using Location Based Routing.  Because all calls could eventually be affected.  So let&#8217;s take this simple example: In the boss/admin scenario where the admin is a delegate of the boss.  If the admin travels to India, in our example above, and is a sim-ring target for PSTN calls to the boss, the admin would not get these calls as it would circumvent location based routing.

I should note that in the reverse where the boss has traveled to India and the admin is located in Minneapolis a inbound call to the boss would not ring her endpoint in India but the Sim Ring delegate in Minneapolis would ring.  If the admin took the call and attempted to transfer the call to the boss in India, the transfer would fail as that would allow the PSTN call to be routed over the WAN.

NOTE: These are on PSTN calls and nothing I have seen or tested would affect a Lync-to-Lync call within the enterprise.

**DR/Pool Failover**

During pool failover all Location Based Routing rules still apply and therefore you need to make sure you carefully understand what the implications are of the two.

**Deployment and Configuration**

Let&#8217;s use the following as an example of our deployments

  * Trunk1 &#8211; Minneapolis-Gateway
  * Trunk2 &#8211; India-Gateway

We have the following voice policies

  * MinneapolisVoicePolicy 
      * PSTN Usage: MSP-Usage
  * IndiaVoicePolicy 
      * PSTN Usage: India-Usage

And our voice routes would be something like:

  * Route 1 &#8211; MSP-Route is associated to MSP-Usage and Minneapolis-Gateway
  * Route 2 &#8211; India-Route is associated to India-Usage and India-Gateway

<span style="text-decoration: underline">UPDATED: Somehow my copy/paste missed this section.  Thanks Elan for pointing this out!</span>

Create a new voice routing policy:

New-CsVoiceRoutingPolicy -Identity &#8220;MSPvoiceroutingpolicy&#8221; -Name &#8220;Minneapolis voice routing policy” -PstnUsages @{add=&#8221;MSP-Usage&#8221;}

<span style="text-decoration: underline">END UPDATE</span>

So on the User Voice Policy you would need to enable this:

Set-CsVoicePolicy MinneapolisVoicePolicy -PreventPSTNTollBypass $True

On the Network Site that has been defined you would need to enable it:

Set-CsNetworkSite -Identity MinneapolisNetworkSite -EnableLocationBasedRouting $true -VoiceRoutingPolicy MSPvoiceroutingpolicy

Configuration of the Trunk also:

Set-CsTrunkConfiguration -Identity Trunk1 -EnableLocationRestriction $true -NetworkSiteID MinneapolisNetworkSite<!--?xml:namespace prefix = "o" ns = "urn:schemas-microsoft-com:office:office" /-->

Lastly, it must be configured globally as enabled as well:

Set-CsRoutingConfiguration -EnableLocationBasedRouting $true<!--?xml:namespace prefix = "o" ns = "urn:schemas-microsoft-com:office:office" /-->

Hopefully that sheds some light on the new Location Based Routing functionality.  I&#8217;ll follow up with a nice Visio format of the above so for anyone who is a visual learner it would make sense as well.

UPDATE: Speaking with Ken Lasko (Lync MVP &#8211; <http://ucken.blogspot.com/>) he will start looking into updating the Lync Optimizer to handle some of these new routing options.