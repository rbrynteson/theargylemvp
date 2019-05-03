---
id: 313
title: Long Distance Codes and Billing
date: 2013-07-05T20:48:56+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=313
permalink: /2013/07/05/long-distance-codes-and-billing/
st_layout_box:
  - right
panels_data:
  - 'a:0:{}'
categories:
  - Lync Server 2013
---
This one is one of those Lync can do it, it&#8217;s pretty hack-ish but it works and the customer is happy so we just go with it type of articles.

So I again ran into a customer who had billing codes for long distant calls.  Typically when we break down the use of these codes we discover that either most people don&#8217;t use them or they don&#8217;t actually get billed back to the client.  However, this was one of those unique ones where it was a small law firm and they used them all the time.

Now there are some third party solutions out there that allow you to do automatic billing or it&#8217;s done at the carrier level but I wanted to see if I could do it in-the-box.  And like so many things in the world of Lync and Enterprise Voice there isn&#8217;t a &#8220;report&#8221; for this information.  So I decided it was time to play around with voice routing and see if I could create something &#8220;similar&#8221; to what they had in the past.

So the requirements are pretty simple:

  1. Users are required to dial some sort of &#8220;code + billing number&#8221; before the call goes out.
  2. Calls to the PSTN obviously can&#8217;t have that billing code.
  3. CDR information must contain the billing code information.
  4. There has to be no cost to this.

So with those out of the way, let&#8217;s play around with Voice Routing rules and SQL and accomplish the task.

**Billing Codes**

So the first thing we need to do is account for billing codes.  I&#8217;m doing this in my lab so in my case I have a very small business and will never have more than 500 clients we need to bill.  So our codes will be as simple as:

xxx

But I want to have some sort of easy to recognize escape code that I might use later on for by SQL query so I&#8217;m going to tell all of my users they must dial a 999 as the billing code requirement.  So my user would then dial:

999xxx17635551234

I picked 999 as there is no country code that starts with those three digits at this time.  Of course, if a new country comes along that fits this, I might need to change some things.  The first thing we do is create a normalization rule for that.

<a href="http://masteringlync.com/2013/07/05/long-distance-codes-and-billing/pic1-5/" rel="attachment wp-att-314"><img class="alignnone wp-image-314 size-full" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/07/pic1.png?resize=375%2C165&#038;ssl=1" alt="pic1" width="375" height="165" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/07/pic1.png?w=375&ssl=1 375w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/07/pic1.png?resize=300%2C132&ssl=1 300w" sizes="(max-width: 375px) 100vw, 375px" data-recalc-dims="1" /></a>

Rule is straight forward.  We simply add a + to the front of the string I&#8217;ve created before.  You will notice I&#8217;m a nice guy and typically allows my users to dial an option 9 because they are used to it.

**Routing**

Now that we have secured our normalization rule, we need to route it out of the system.  So I&#8217;ll take my existing route and modify it for this new normalization pattern.

<a href="http://masteringlync.com/2013/07/05/long-distance-codes-and-billing/pic2-5/" rel="attachment wp-att-315"><img class="alignnone wp-image-315 size-full" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/07/pic2.png?resize=426%2C106&#038;ssl=1" alt="pic2" width="426" height="106" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/07/pic2.png?w=426&ssl=1 426w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/07/pic2.png?resize=300%2C75&ssl=1 300w" sizes="(max-width: 426px) 100vw, 426px" data-recalc-dims="1" /></a>

This rule is again pretty straight forward.  We are looking for an optional 999xxx followed by the traditional North America dialing.  If we were going to support international dialing I would update my international dialing route as well.

**Trunk Configuration**

I don&#8217;t want to have to do any work on my gateway (because they should be set them and forget them devices) so I&#8217;m going to use trunk configuration to strip out my 999xxx (and my +) before I send it to the gateway.  So again, nothing amazing in terms of rules:

<a href="http://masteringlync.com/2013/07/05/long-distance-codes-and-billing/pic3-4/" rel="attachment wp-att-316"><img class="alignnone size-full wp-image-316" src="https://i0.wp.com/masteringlync.com/files/2013/07/pic3.png?resize=371%2C162&#038;ssl=1" alt="pic3" width="371" height="162" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/07/pic3.png?w=371&ssl=1 371w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/07/pic3.png?resize=300%2C131&ssl=1 300w" sizes="(max-width: 371px) 100vw, 371px" data-recalc-dims="1" /></a>

This rule simply finds any number that starts with +999xxx and removes all of that and sends out just the remaining digits.

**Call Someone**

In my Lync client, I can simply dial 99912317635551234 and I find it will normalize to this:

<a href="http://masteringlync.com/2013/07/05/long-distance-codes-and-billing/pic4-4/" rel="attachment wp-att-317"><img class="alignnone wp-image-317 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/07/pic4.png?resize=262%2C141&#038;ssl=1" alt="pic4" width="262" height="141" data-recalc-dims="1" /></a>

And the call is successful to my cell phone.  So I have accomplished what my goal is to get outbound dialing.  However, now I need to actually get this data to the billing person.  As you can tell, this was the easy part of my task, but what does this information look like in the reporting server?

<a href="http://masteringlync.com/2013/07/05/long-distance-codes-and-billing/pic5-4/" rel="attachment wp-att-318"><img class="alignnone wp-image-318 size-full" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/07/pic5.png?resize=800%2C64&#038;ssl=1" alt="pic5" width="800" height="64" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/07/pic5.png?w=1171&ssl=1 1171w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/07/pic5.png?resize=300%2C24&ssl=1 300w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/07/pic5.png?resize=768%2C62&ssl=1 768w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/07/pic5.png?resize=1024%2C82&ssl=1 1024w" sizes="(max-width: 800px) 100vw, 800px" data-recalc-dims="1" /></a>

So we can clearly see we have the data we need but make this easier for our billing person.  I mean, we could give her the SRS reporting page and export this to CSV from it but if you have ever done that you know it&#8217;s a mess.  So instead, let&#8217;s go to SQL and see if I can put together a better query.

Spending a little time in SQL you can find this query will most likely get you everything you want:

<pre>SELECT SessionDetailsView.SessionIdTime, SessionDetailsView.SessionIdSeq, SessionDetailsView.FromUri, SessionDetailsView.ToUri, 
SessionDetailsView.TargetUri, SessionDetailsView.OnBehalfOfUri, SessionDetailsView.InviteTime, SessionDetailsView.ResponseTime, SessionDetailsView.EndTime, DATEDIFF(MINUTE, SessionDetailsView.InviteTime, SessionDetailsView.EndTime) AS TimeToBillMin, SUBSTRING(TargetUri, 5, 3) AS CustNumber ,MediaList.Media
FROM  SessionDetailsView INNER JOIN Media ON SessionDetailsView.SessionIdTime = Media.SessionIdTime INNER JOIN MediaList ON Media.MediaId = MediaList.MediaId
WHERE (SessionDetailsView.SessionIdTime &gt;= '2013-07-01') AND (SessionDetailsView.SessionIdTime &lt;= '2013-07-31') AND (MediaList.Media = 'Audio') AND (TargetUri LIKE '+999%')</pre>

In my SQL query there are a few things you will need to modify depending on your environment.

First, if your &#8220;client code&#8221; is different than three characters then you will need to modify the SUBSTRING(TargetUri, 5, 3) to the correct length.

Second, if you decide to use something different than 999 as your code, you need to update that in two different places in the query.

Third, obviously, you would modify the SessionIdTime based on when your calls actually happened.

And what this will return in SQL is this:

<a href="http://masteringlync.com/2013/07/05/long-distance-codes-and-billing/pic6-4/" rel="attachment wp-att-319"><img class="alignnone wp-image-319 size-full" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/07/pic6.png?resize=800%2C50&#038;ssl=1" alt="pic6" width="800" height="50" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/07/pic6.png?w=1527&ssl=1 1527w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/07/pic6.png?resize=300%2C19&ssl=1 300w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/07/pic6.png?resize=768%2C48&ssl=1 768w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/07/pic6.png?resize=1024%2C64&ssl=1 1024w" sizes="(max-width: 800px) 100vw, 800px" data-recalc-dims="1" /></a>

And there you go.  I&#8217;ll play around with Excel later this weekend and maybe create an excel spreadsheet that has the query defined in it and all you need to do is enter the needed information &#8211; but I&#8217;m not an Excel wiz so it might be a bit of work.

Lastly, there are a few items of note:

First, I have not done extensive testing on this process yet.  I will continue to refine it based on feedback I get the client and if anyone else happens to note anything.

Second, this has only been tested against Lync Server 2013 CU1 at this time.  I&#8217;ll do more testing and validate no views have changed from RTC to CU1 to CU2.

Third, I did some hunting of the inner-webs before typing this up and didn&#8217;t find any articles that were similar.  However, if someone else has a better way to accomplish this by all means let me know.  As I said, this was a little hack-ish in nature.

&nbsp;