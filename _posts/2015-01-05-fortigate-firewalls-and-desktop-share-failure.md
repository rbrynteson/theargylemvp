---
id: 914
title: Fortigate Firewalls and Desktop Share Failure
date: 2015-01-05T14:28:45+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=914
permalink: /2015/01/05/fortigate-firewalls-and-desktop-share-failure/
st_layout_box:
  - right
panels_data:
  - 'a:0:{}'
image: https://masteringlync.com/wp-content/uploads/sites/2/2015/01/4.png
categories:
  - Lync Server 2010
  - Lync Server 2013
---
I recently ran into a problem with Desktop Sharing and Lync.  Typically, I start all of these troubleshooting sessions by looking at the common issues: certs, DNS, etc. but in this case I knew who to blame.  Me!

I recently installed a new firewall into the environment: A Fortigate 50b.  Before we get to the specific fix lets review what was and wasn&#8217;t working.

  * Presence worked both ways
  * IM&#8217;s worked both ways
  * Audio/Video worked both ways
  * Desktop Share would fail on setup

With this knowledge, I could start making some very good guesses of what might be happening.  Since Desktop Share utilizes TCP exclusively and Audio/Video uses UDP, I had a good guess that the problem lied somewhere in the TCP stack on the firewall.

So my next step was to review a client trace to see if there was anything in the client trace that would point me to any more specific information.  A review of the SDP showed this:

[<img class="alignnone wp-image-915 size-full" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/01/1.png?resize=639%2C286&#038;ssl=1" alt="" width="639" height="286" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/01/1.png?w=639&ssl=1 639w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/01/1.png?resize=300%2C134&ssl=1 300w" sizes="(max-width: 639px) 100vw, 639px" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/01/1.png)

The workstation I am running the client on has two NIC&#8217;s (one wired and one wireless) so we see both of those IP&#8217;s listed but you will notice a lack of any relay/edge addresses.  So we can see that somewhere in the process of the TURN request, something was deleting, removing, preventing the TURN request.  Since this is a TCP request, we know the request is going out on 443.

So I decided to start poking around the Fortigate and see if there was any type of TCP/443 filtering that might be enabled and I found Unified Threat Management (UTM).  After I got over the shivers of the name I decided to disable UTM on my firewall policy.  My client network is VLAN4 on my Fortigate so I disabled UTM on VLAN 4 to WAN 1:

[<img class="alignnone wp-image-916 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/01/2.png?resize=696%2C30&#038;ssl=1" alt="" width="696" height="30" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/01/2.png?w=696&ssl=1 696w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/01/2.png?resize=300%2C13&ssl=1 300w" sizes="(max-width: 696px) 100vw, 696px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/01/2.png)

[<img class="alignnone wp-image-917 size-medium" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/01/3-300x129.png?resize=300%2C129&#038;ssl=1" alt="" width="300" height="129" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/01/3.png?resize=300%2C129&ssl=1 300w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/01/3.png?w=555&ssl=1 555w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/01/3.png)

After the change, I immediately tested my desktop share and it was working.  At this point, I started disabling each option one at a time to discover the issue was related to the Protocol Options being enabled.  So I looked at the details this can be found in Firewall | Policy | Protocol Options:

[<img class="alignnone wp-image-918 size-medium" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/01/4-300x144.png?resize=300%2C144&#038;ssl=1" alt="" width="300" height="144" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/01/4.png?resize=300%2C144&ssl=1 300w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/01/4.png?w=628&ssl=1 628w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/01/4.png)

My attention went immediately to the HTTPS section.  I found that if I disabled Monitor Content Information for Dashboard, Desktop Share starts to work immediately.

**More Details**

When I ran into this problem I knew this might be one of those issues that might make a great blog article.  So I did some quick searching and found this blog article:

<http://silbers.net/blog/2013/02/04/application-layer-firewall-blocks-lync-application-sharing/>

So although I started a blog post with some pretty WireShark images &#8211; I try to never double up another persons content.  As my old debate coach would say: &#8220;Stealing content just isn&#8217;t cool, dude!&#8221;  So if you want some more great content, go read Jeremy Silber&#8217;s blog on the topic.

&nbsp;