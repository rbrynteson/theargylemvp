---
id: 1283
title: 'Quick Tip: Polycom VVX Phones &amp; Configured Source Preference'
date: 2016-09-12T13:29:28+00:00
author: Richard Brynteson
layout: post
guid: http://masteringlync.com/?p=1283
permalink: /2016/09/12/quick-tip-polycom-vvx-phones-configured-source-preference/
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
  - Skype for Business
---
Another day and another odd ball item when it comes to desk phones.  I don&#8217;t think this one is on Polycom in particular but it&#8217;s interesting none the less.

**Background Info**

One of the really neat features of a Polycom VVX series is the ability to manage these devices remotely.  You could use a traditional Polycom Provisioning Server concept (see <a href="http://blog.schertz.name/2013/05/provisioning-polycom-sip-phones/" target="_blank">Jeff&#8217;s post on details</a>), you could purchase a third party product like EventZero&#8217;s Provisioning Tool (see <a href="https://www.eventzero.com/Provisioning/ProvisioningPolycom/" target="_blank">product page</a>) or you can do your own thing and host it in Azure (like I did with the <a href="http://skypevalidator.com/" target="_blank">SkypeValidator</a>).

However you decide to manage your phones the key piece is that you can centrally manage those phones, get details logs, etc.  This is fantastic when it works like you think it should.

For the phones we have four ways we can apply settings:  Local (at the phone), Web (Web GUI), Config (provisioning server) and SIP (pushed from your SIP Server).

When you hover over a setting from the Web GUI, it will actually show you everything that is configured and from which source.  This is very neat.

<a href="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/09/1.png" rel="attachment wp-att-1284"><img class="alignnone size-medium wp-image-1284" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/09/1-300x180.png?resize=300%2C180&#038;ssl=1" alt="1" width="300" height="180" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/09/1.png?resize=300%2C180&ssl=1 300w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/09/1.png?w=314&ssl=1 314w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" /></a>

You might ask yourself, what is the order in which these are applied.  Who gets preference when.  Well, according to the docs the order is Local, Web and Config &#8211; with Local being the most preferred and Config being the least preferred.  Makes complete sense.

**What about SIP? **

That is a good question, when we reached out to Polycom they said when SIP is involved that is takes the lowest priority.  Except when it doesn&#8217;t.

Huh?

That was my reaction as well.  So when doesn&#8217;t it take priority?  Well, one instance of this is the Message Center.  This controls where the phone goes when you click the Message Button (i.e. VoiceMail) on the phone.  For me, we have a customer who cannot use Exchange VM with their Skype for Business solution for regulatory reasons.  OK, easy enough, there are a bunch of great 3rd Party VM solutions.  We forward phones to that location on a non-answer via SefaUtil.  It&#8217;s not a perfect solution but it works and gets around any legal issues.

So we went to our Polycom Provisioning Server, set the msg.mwi.1.callBack setting to the phone number contact and we should be good.  Check the config, should be good, nope.

<a href="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/09/2.png" rel="attachment wp-att-1285"><img class="alignnone size-medium wp-image-1285" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/09/2-300x190.png?resize=300%2C190&#038;ssl=1" alt="2" width="300" height="190" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/09/2.png?resize=300%2C190&ssl=1 300w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/09/2.png?w=334&ssl=1 334w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" /></a>

When we press the Message button on the phone it still calls the URI set by the SIP Server.  We reboot the phone a dozen times, nothing.  We went back, factory reset the phone and ONLY pushed the mwi setting and had the same problem.

This is one of those &#8220;special&#8221; settings that the config server doesn&#8217;t override SIP.

If we go into the WebGUI and copy/paste the same exact setting we are pushing out via the Provisioning Server and apply, now the Message Center button goes to the right place.

So the moral of the story is, Provisioning Servers are really cool when they do everything you want, but sometimes they don&#8217;t.