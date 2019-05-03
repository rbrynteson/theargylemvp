---
id: 1278
title: 'Quick Tip: Polycom VVX Phones &amp; SLA'
date: 2016-08-12T17:04:06+00:00
author: Richard Brynteson
layout: post
guid: http://masteringlync.com/?p=1278
permalink: /2016/08/12/quick-tip-polycom-vvx-phones-sla/
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
  - Skype for Business
---
So if you have been following my blog you know that I&#8217;m a huge fan of Shared Line Appearance (SLA) that was added to Skype for Business and later brought back to Lync 2013.  We use it so much, that almost every Response Group deployed is gone.  In some cases, we use RGS simply for time of day routing and then send it to an SLA, but regardless never using the actual RGS queuing process.

However, today we ran into a very odd issue.  We had a client whose phones would not answer an SLA call when you lifted the handset.  You had to physically push the button (on the VVX 3xx and 4xx) or jump through some serious hoops on the VVX 5xx and 6xx devices to answer the call.  The phone would pretend that you wanted to make a new call.

After looking at server logs and all sorts of random things we went back to the phones.

For some reason (someone from Polycom will need to explain why) the phones come defaulted to SLA Ring Type of &#8220;Silent Ring&#8221;.  Now we remind clients all the time to make sure they set that (or use a provisioning server and set it) but sometimes phones get missed, or in our case, we did a factory reset to try to solve another issue.

If on the phones, you go to Settings | Basic | Ring Type | SLA Ring Type you will see this:

<a href="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/08/1.png" rel="attachment wp-att-1279"><img class="alignnone size-medium wp-image-1279" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/08/1-300x225.png?resize=300%2C225&#038;ssl=1" alt="1" width="300" height="225" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/08/1.png?resize=300%2C225&ssl=1 300w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/08/1.png?w=319&ssl=1 319w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" /></a>

Here is the kicker, if you have this set to Silent Ring, when you lift the receiver the SLA call will not be answered, and you have to click the button.  If you have it set to anything other than Silent Ring then when you lift the handset, an SLA will be answered just like any other call on the planet.

Again, I don&#8217;t know if this is a design decision from Polycom, a mandate from Microsoft, but regardless it&#8217;s not intuitive and certainly wasn&#8217;t what we expected.

We tested this in all flavors of 5.4.3 and 5.4.4 software and it was there.  Maybe when 5.5.1 comes out with official SfB support I&#8217;ll update this if anything changes.  Or again, maybe people really wanted this feature, but haven&#8217;t figured out why yet.