---
id: 288
title: Inter Trunk Routing Deep Dive
date: 2013-06-07T03:57:33+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=288
permalink: /2013/06/07/inter-trunk-routing-deep-dive/
st_layout_box:
  - right
panels_data:
  - 'a:0:{}'
categories:
  - Lync Server 2013
  - Uncategorized
---
In our previous three posts about understanding voice routing we investigated outbound dialing from both the Dialing Behavior, Routing & Authorization levels and In-Bound Routing. In this post we are going to explore Inter-Trunk routing or as Microsoft now refers to it as Basic Session Management.

  * Investigating Dialing – [Dialing Behaviors](http://masteringlync.com/2013/04/07/understanding-voice-routing-dialing-behaviors/)
  * Investigating Dialing – [Routing & Authorization](http://masteringlync.com/2013/04/11/understanding-voice-routing-routing-authorization/)
  * Investigating Dialing – [Dialing Behaviors of an Inbound Call](http://masteringlync.com/2013/05/31/understanding-voice-routing-inbound-routing/) 
  * Investigating Dialing – [Inter Trunk Dialing](http://masteringlync.com/2013/06/06/inter-trunk-routing-deep-dive/) &#8211; This Post

PRE-POST-UPDATE: So good friend Ken Lasko has stolen a little of my thunder in his recent [blog post](http://ucken.blogspot.com/2013/06/inter-trunk-routing-in-lync-2013.html#comment-form).  Don&#8217;t want anyone thinking I just ripped off his content.  We will do much of the same work however we will dive a little deeper into the guts and see exactly what is happening here.

**Scenario:**

Unlike the other series we are going to have to setup a specific scenario that we want investigate in order to be able to fully understand what is happening here.  So I have created the following setup in my environment.

<a href="http://masteringlync.com/2013/06/07/inter-trunk-routing-deep-dive/pic1-4/" rel="attachment wp-att-289"><img class="alignnone wp-image-289 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/06/pic1.png?resize=503%2C513&#038;ssl=1" alt="pic1" width="503" height="513" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/06/pic1.png?w=503&ssl=1 503w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/06/pic1.png?resize=294%2C300&ssl=1 294w" sizes="(max-width: 503px) 100vw, 503px" data-recalc-dims="1" /></a>

We have four phone numbers that I have carved out of my DID block to play with.  2820 to 2823.  A quick description of each of the gateways, trunks, etc.

  * Gateway 10.12.4.16 is an AudioCodes Gateway with PRI  <span style="color: #ff0000">(GW-PSTN)</span>
  * Lync 2013 Mediation Server is within the (corp.com) domain. <span style="color: #ff0000">(Lync)</span>
  * Gateway 10.12.34.26 is an I3 Call Center System <span style="color: #ff0000">(GW-CIC)</span>
  * Gateway 10.12.17.166 is a Lync 2013 Standard Edition Server in a different Active Directory Forest (thelab.info domain). <span style="color: #ff0000">(GW-LAB)</span>

My goal is to get four digit dialing from the PSTN (10.12.4.16) to the Lync 2013 Standard Edition Server (10.12.17.166).  Likewise, I want to be able to outbound dial from my Lync 2013 Standard Edition Server and reach CIC, the other Lync Server Domain and the PSTN.  I decided to use a Lync Server as my third &#8220;gateway&#8221; so I could use CLS/Snooper and output familiar looking logs.

For the purposes of this example I&#8217;ll refer to each component as the GW name listed above in red to make everything easy to keep track of.

**Configuration &#8211; Inbound Routing from PSTN**

We will start by looking first at the inbound routing process for inter-trunk routing.  You will discover this configuration can get confusing very quickly.

<span style="text-decoration: underline">Topology Builder Changes</span>

We need to ensure that PSTN Gateways exist in Lync for each of the three Gateways.  We do this via topology builder.  There is nothing special or unique about this configuration.  In all three of my gateways I have decided to use port 5060 and all gateways are configured as TCP to make tracing easier.

<span style="text-decoration: underline">Control Panel Changes</span>

This is where the most complicated part of the configuration must take place.  Inter-trunk routing is based on the idea that after going through the call flow process, we match another route and route the call down that path.  The trick becomes authorization.  For a normal Lync user this is done via the voice policy and PSTN usages.  For inter-trunk routing this is done via the Trunk Configuration and PSTN Usages.

So the first thing we must do is create routes for our calls.  So we know that we want 2821 to route to GW-CIC and 282[2-3] to route to GW-LAB.  That leads me to create these two routes:

<a href="http://masteringlync.com/2013/06/07/inter-trunk-routing-deep-dive/pic2-4/" rel="attachment wp-att-290"><img class="alignnone wp-image-290 size-full" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/06/pic2.png?resize=634%2C63&#038;ssl=1" alt="pic2" width="634" height="63" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/06/pic2.png?w=634&ssl=1 634w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/06/pic2.png?resize=300%2C30&ssl=1 300w" sizes="(max-width: 634px) 100vw, 634px" data-recalc-dims="1" /></a>

NOTE: You can see my route to Test CIC has both 2820 and 2821.  I&#8217;m doing this on purpose to reinforce the routing process in Lync.  Remember, that 2820 is currently assigned to a user in my Lync environment.

The second item I need to do is create PSTN Usages that will contain these routes.  So I create two of those:

<a href="http://masteringlync.com/2013/06/07/inter-trunk-routing-deep-dive/pic3-3/" rel="attachment wp-att-291"><img class="alignnone wp-image-291 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/06/pic3.png?resize=749%2C66&#038;ssl=1" alt="pic3" width="749" height="66" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/06/pic3.png?w=749&ssl=1 749w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/06/pic3.png?resize=300%2C26&ssl=1 300w" sizes="(max-width: 749px) 100vw, 749px" data-recalc-dims="1" /></a>

I have two usages, named **Inter-Trunk Route to BLM Lab** and **Inter-Trunk Route to CIC**.  You will also notice by the very blank space to the right that these PSTN usages are NOT assigned to any voice policies at this time.  <span style="text-decoration: underline">For the purposes of in-bound routing there is absolutely no need to associate these PSTN usages to a voice policy.</span>

The last step is authorization.  Remember, before I said that inter-trunk routing authorization is done at the trunk configuration level.  So I go to my Trunk Configuration for PSTN-GW (10.12.4.16).  If you do not have a trunk configuration for this gateway you must create a new pool-level trunk config and select this gateway.

<a href="http://masteringlync.com/2013/06/07/inter-trunk-routing-deep-dive/pic4-3/" rel="attachment wp-att-292"><img class="alignnone wp-image-292 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/06/pic4.png?resize=703%2C287&#038;ssl=1" alt="pic4" width="703" height="287" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/06/pic4.png?w=703&ssl=1 703w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/06/pic4.png?resize=300%2C122&ssl=1 300w" sizes="(max-width: 703px) 100vw, 703px" data-recalc-dims="1" /></a>

That is it!  For the purposes of inbound routing there is absolutely no need to assign these PSTN usages to either of the GW-CIC or the GW-LAB trunks in Lync.  You might be asking yourself, why is this, well let&#8217;s go to the logs and see what we get.

1. We get the normal inbound four digit number.

<a href="http://masteringlync.com/2013/06/07/inter-trunk-routing-deep-dive/pic5-3/" rel="attachment wp-att-294"><img class="alignnone wp-image-294 size-full" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/06/pic5.png?resize=800%2C41&#038;ssl=1" alt="pic5" width="800" height="41" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/06/pic5.png?w=927&ssl=1 927w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/06/pic5.png?resize=300%2C15&ssl=1 300w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/06/pic5.png?resize=768%2C39&ssl=1 768w" sizes="(max-width: 800px) 100vw, 800px" data-recalc-dims="1" /></a>

2. We normalize our four digit number into E164

<a href="http://masteringlync.com/2013/06/07/inter-trunk-routing-deep-dive/pic6-3/" rel="attachment wp-att-295"><img class="alignnone wp-image-295 size-full" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/06/pic6.png?resize=800%2C35&#038;ssl=1" alt="pic6" width="800" height="35" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/06/pic6.png?w=1007&ssl=1 1007w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/06/pic6.png?resize=300%2C13&ssl=1 300w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/06/pic6.png?resize=768%2C34&ssl=1 768w" sizes="(max-width: 800px) 100vw, 800px" data-recalc-dims="1" /></a>

3. Now we get a new retargeting event

<a href="http://masteringlync.com/2013/06/07/inter-trunk-routing-deep-dive/pic7-2/" rel="attachment wp-att-296"><img class="alignnone wp-image-296 size-full" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/06/pic7.png?resize=800%2C42&#038;ssl=1" alt="pic7" width="800" height="42" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/06/pic7.png?w=888&ssl=1 888w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/06/pic7.png?resize=300%2C16&ssl=1 300w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/06/pic7.png?resize=768%2C41&ssl=1 768w" sizes="(max-width: 800px) 100vw, 800px" data-recalc-dims="1" /></a>

4. Then we get a new event of creating an outbound routing object.

<a href="http://masteringlync.com/2013/06/07/inter-trunk-routing-deep-dive/pic8-2/" rel="attachment wp-att-297"><img class="alignnone wp-image-297 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/06/pic8.png?resize=800%2C35&#038;ssl=1" alt="pic8" width="800" height="35" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/06/pic8.png?w=964&ssl=1 964w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/06/pic8.png?resize=300%2C13&ssl=1 300w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/06/pic8.png?resize=768%2C33&ssl=1 768w" sizes="(max-width: 800px) 100vw, 800px" data-recalc-dims="1" /></a>

5. Then we see it apply an inter-trunk rule

<a href="http://masteringlync.com/2013/06/07/inter-trunk-routing-deep-dive/pic9-2/" rel="attachment wp-att-298"><img class="alignnone wp-image-298 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/06/pic9.png?resize=800%2C30&#038;ssl=1" alt="pic9" width="800" height="30" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/06/pic9.png?w=1112&ssl=1 1112w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/06/pic9.png?resize=300%2C11&ssl=1 300w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/06/pic9.png?resize=768%2C29&ssl=1 768w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/06/pic9.png?resize=1024%2C39&ssl=1 1024w" sizes="(max-width: 800px) 100vw, 800px" data-recalc-dims="1" /></a>

6. Now we see the trunk is selected

<a href="http://masteringlync.com/2013/06/07/inter-trunk-routing-deep-dive/pic10-2/" rel="attachment wp-att-299"><img class="alignnone wp-image-299 size-full" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/06/pic10.png?resize=800%2C31&#038;ssl=1" alt="pic10" width="800" height="31" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/06/pic10.png?w=1071&ssl=1 1071w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/06/pic10.png?resize=300%2C12&ssl=1 300w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/06/pic10.png?resize=768%2C30&ssl=1 768w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/06/pic10.png?resize=1024%2C40&ssl=1 1024w" sizes="(max-width: 800px) 100vw, 800px" data-recalc-dims="1" /></a>

7. And lastly we see the route is selected.

<a href="http://masteringlync.com/2013/06/07/inter-trunk-routing-deep-dive/pic11-2/" rel="attachment wp-att-300"><img class="alignnone wp-image-300 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/06/pic11.png?resize=784%2C39&#038;ssl=1" alt="pic11" width="784" height="39" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/06/pic11.png?w=784&ssl=1 784w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/06/pic11.png?resize=300%2C15&ssl=1 300w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/06/pic11.png?resize=768%2C38&ssl=1 768w" sizes="(max-width: 784px) 100vw, 784px" data-recalc-dims="1" /></a>

So now that we see what is happening on the Lync Server, what do we get on the GW-LAB servers for incoming messaging?  If we do a quick trace there we can see this:

So the inbound call is coming in as E614.

<a href="http://masteringlync.com/2013/06/07/inter-trunk-routing-deep-dive/pic20/" rel="attachment wp-att-305"><img class="alignnone size-full wp-image-305" src="https://i1.wp.com/masteringlync.com/files/2013/06/pic20.png?resize=784%2C39&#038;ssl=1" alt="pic20" width="784" height="39" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/06/pic20.png?w=784&ssl=1 784w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/06/pic20.png?resize=300%2C15&ssl=1 300w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/06/pic20.png?resize=768%2C38&ssl=1 768w" sizes="(max-width: 784px) 100vw, 784px" data-recalc-dims="1" /></a>

This makes sense because we normalized the number to E164 back in Lync.  That number was determined to be used for inter-trunk routing because of our PSTN usage applied to the GW-PSTN.  But remember, our scenario needs us to send four digits to GW-LAB.  So in order to accomplish this, we need to go back to our Lync server and create a new PSTN Trunk for GW-LAB.

So here you can our new trunk and rules:

<a href="http://masteringlync.com/2013/06/07/inter-trunk-routing-deep-dive/pic21/" rel="attachment wp-att-306"><img class="alignnone wp-image-306 size-full" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/06/pic21.png?resize=674%2C226&#038;ssl=1" alt="pic21" width="674" height="226" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/06/pic21.png?w=674&ssl=1 674w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/06/pic21.png?resize=300%2C101&ssl=1 300w" sizes="(max-width: 674px) 100vw, 674px" data-recalc-dims="1" /></a>

So we run the trace again:

<a href="http://masteringlync.com/2013/06/07/inter-trunk-routing-deep-dive/pic22/" rel="attachment wp-att-307"><img class="alignnone wp-image-307 size-full" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/06/pic22.png?resize=800%2C33&#038;ssl=1" alt="pic22" width="800" height="33" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/06/pic22.png?w=969&ssl=1 969w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/06/pic22.png?resize=300%2C12&ssl=1 300w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/06/pic22.png?resize=768%2C32&ssl=1 768w" sizes="(max-width: 800px) 100vw, 800px" data-recalc-dims="1" /></a> <a href="http://masteringlync.com/2013/06/07/inter-trunk-routing-deep-dive/pic23/" rel="attachment wp-att-308"><img class="alignnone wp-image-308 size-full" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/06/pic23.png?resize=800%2C34&#038;ssl=1" alt="pic23" width="800" height="34" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/06/pic23.png?w=940&ssl=1 940w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/06/pic23.png?resize=300%2C13&ssl=1 300w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/06/pic23.png?resize=768%2C33&ssl=1 768w" sizes="(max-width: 800px) 100vw, 800px" data-recalc-dims="1" /></a>

And now we are sending the four digits to GW-LAB as we needed.  So although the documentation is a little thin around this (meaning there is next to none) it is very possible to front end all of your PSTN traffic with Lync.  We will do a part two on this for cover the other parts of the scenario, outbound routing and such.  But frankly, that part isn&#8217;t all that difficult.

&nbsp;