---
id: 282
title: Understanding Voice Routing | Inbound Routing
date: 2013-05-31T15:36:56+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=282
permalink: /2013/05/31/understanding-voice-routing-inbound-routing/
st_layout_box:
  - right
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
  - Lync Server 2013
---
In our previous two posts about understanding voice routing we investigated outbound dialing from both the Dialing Behavior and Routing & Authorization levels. In this post we are going to explore Inbound Routing.

&nbsp;

  * Investigating Dialing – [Dialing Behaviors](http://masteringlync.com/2013/04/07/understanding-voice-routing-dialing-behaviors/)
  * Investigating Dialing – [Routing & Authorization](http://masteringlync.com/2013/04/11/understanding-voice-routing-routing-authorization/)
  * Investigating Dialing – [Dialing Behaviors of an Inbound Call](http://masteringlync.com/2013/05/31/understanding-voice-routing-inbound-routing/) &#8211; this post
  * Investigating Dialing – [Inter Trunk Dialing](http://masteringlync.com/2013/06/07/inter-trunk-routing-deep-dive/)

Overview

One of the facts that often goes over locked is that Lync can do inbound normalization of calls from the PSTN. All too often I see administrators setup all of the normalization rules on the gateway and although this works you should have noticed in the first post that certain aspects of the routing engine are skipped and this may not be desirable. Furthermore, its important to realize that inbound normalization applies to both the calling number and called number &#8211; even in Lync 2010. So although Lync 2010 didn&#8217;t support outbound manipulation of the calling number, it does support inbound manipulation.

First off, let&#8217;s look at the call flow slide again in case you have forgotten.

<a href="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/04/voice1_thumb1.jpg" rel="attachment wp-att-1034"><img class="alignnone size-full wp-image-1034" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/04/voice1_thumb1.jpg?resize=687%2C378&#038;ssl=1" alt="voice1_thumb" width="687" height="378" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/04/voice1_thumb1.jpg?w=687&ssl=1 687w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/04/voice1_thumb1.jpg?resize=300%2C165&ssl=1 300w" sizes="(max-width: 687px) 100vw, 687px" data-recalc-dims="1" /></a>

&nbsp;

In this example we will pretend that our PSTN Provider is sending us four digits of 2820. What exactly happens in our inbound call flow. Make sure you have read the first article on Dialing Behaviors as I&#8217;m going to skip through certain sections quite quickly as they have been covered.

**User Initiates Call**

Remember, the flow chart from above is for both inbound and outbound calls so it doesn&#8217;t matter that this is an inbound call, we still say the User Initiates the Call. We do our check to determine if the call is a SIP URI, if it is not we move onto the next step. Since the inbound number is 2820, that is not a valid SIP URI so we move on.

**Emergency Call**

In this step we look at the number being presented and determine if the call is an emergency call. Recall that our emergency numbers are set by the location profile assigned to the user and in this case is 911 or 211 and in both cases they do not match our inbound number of 2820. So we will move onto the next step.

**Dial Plan & Normalization**

We have finally arrived at the Dial Plan and normalization rule section. Here we are going to use the Dial Plan assigned to the Trunk (in Lync 2013) or Gateway (in Lync 2010) to determine what to do with the inbound number. So our dial plan could be assigned one of three ways to this trunk – via the global dial plan, a site dial plan or a pool dial plan. Often times I like to use Site or Pool Dial Plans as I might have specific normalization I&#8217;m going to do with the inbound call that I may not want to normalize to other users in the organization. So although a global dial plan will work it really is recommended to create at least a site level dial plan.

I personally prefer to create Pool level dial plans as I might desire the granular nature of rules for each trunk/gateway within a site. However, this does mean that if I had two PSTN trunks/gateways in the same site and I made an administrative change, I will need to make the change twice on each pool dial plan. Again, this is a preference issue more than anything else.

So in our example, we have pool dial plan defined for our trunk. And if I look at the dial plan rules I will see the following rules:

&nbsp;

(I know my screenshot doesn&#8217;t have the rules committed &#8211; trust me they are there.)

Again, we can use the InboundAndOutgoingCall Scenario to trace this call and looking at the messages tab of Snooper get detailed information to exactly what rules are being matched.

<a href="http://masteringlync.com/2013/05/31/understanding-voice-routing-inbound-routing/inbound1/" rel="attachment wp-att-286"><img class="alignnone wp-image-286 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/05/inbound1.png?resize=800%2C65&#038;ssl=1" alt="inbound1" width="800" height="65" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/05/inbound1.png?w=1022&ssl=1 1022w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/05/inbound1.png?resize=300%2C24&ssl=1 300w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/05/inbound1.png?resize=768%2C62&ssl=1 768w" sizes="(max-width: 800px) 100vw, 800px" data-recalc-dims="1" /></a>

So in this case we can see the call is being normalized to +19529122820. The interesting aspect I want to call out here is that Lync will normalize both the called number (2820) and the calling number (952–555–3333). So our goal as always is to get both numbers formatted into an E164 format.

However, if our dial plan did not contain a rule for 2820 than we would return a 404: No Matching Rule found here.

Once we have normalized all of our numbers to E164 we can move onto the next step.

**Call Park Orbit Range**

Believe it or not, we also check the inbound call park orbit range even on inbound calls from the PSTN. Remember the guidelines for call park numbers in Lync, they should be a range of numbers that are not normalized within your environment and are reserved specifically for this purpose. I often times will use ranges that start with # to ensure that the range I&#8217;m using will never be normalized. So if we were using #100 to #149 for example, our new normalized number into E164 does not match and therefore we would move onto to the next step.

However, before we move onto that next step, lets talk about an interesting and available use case scenario that you might want to use. I have seen this used only once before in a medical field but you might find it helpful. Did you know that you can use DID numbers as an inbound call park orbit range? You certainly can it just takes a little bit of effort to make the process work.

Most certainly an interesting sidebar to how routing works but let&#8217;s continue our call now that our number has been normalized to E164.

**Reverse Number Lookup**

Now that we have a valid E164 number we are going to try RNL for the user. So the system will search it&#8217;s internal database looking for an exact match for our Line URI of tel:+19529122820 and in this case we will find our user and route the call.

Now there are two interesting items about this step.

First is remember that RNL is looking for an exact match. Have ever experienced a 408 Ambiguous error in a SIP message? That happens when RNL returns more than one exact number. This most often times happens in one of two scenarios:

  * Administrator A enters a LineURI of a user as tel:+19525551111 and Administrator B enters a LineURI of tel:+19525551111;ext=1111. From the control panel and PowerShell perspective these numbers are different and will be saved successfully into the system. However, from an RNL perspective these two numbers are exactly the same as RNL is matching everything up to the ; and strips the ;ext=xxxx of the number.
  * When using an internal extension range you might have a set of common area phones with the LineURIoftel:=19525551112;ext=6001, 6002, etc. but when someone dials the DID of 19525551112 if you don&#8217;t have something ready to deal with this scenario the system will match all of these numbers and our call will fail. In this scenario you would want to have a dial plan rule to normalize 1112 not to +19525551112 but instead +19525551111;ext=0 where that number is tied to an Exchange Auto Attendant or another user.

Second, what happens if there isn&#8217;t an RNL match? In Lync 2010 that call would continue into the routing engine and fail. However, in Lync 2013, this is how Inter-Trunk routing (or session management as it is sometimes referred to as) works and that will be our next post.