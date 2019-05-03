---
id: 1173
title: 'Quick Tip: No Dial Plans available in Exchange 365 / Voice Mail'
date: 2015-07-01T13:49:53+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=1173
permalink: /2015/07/01/quick-tip-no-dial-plans-available-in-exchange-365-voice-mail/
panels_data:
  - 'a:0:{}'
categories:
  - Exchange Online
  - Lync Server 2013
  - Skype for Business
---
**Problem**: You have configured your Hosted Voicemail Policy to work with Office 365/Exchange Online and Skype for Business (Lync) On-Prem.  We do this by creating a policy that looks like this:

[<img class="alignnone wp-image-1174 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/07/1.png?resize=391%2C62&#038;ssl=1" alt="1" width="391" height="62" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/07/1.png?w=391&ssl=1 391w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/07/1.png?resize=300%2C48&ssl=1 300w" sizes="(max-width: 391px) 100vw, 391px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/07/1.png)

When we would call the Exchange Auto-Attendant hosted in O365 the call would complete fine.  However, we found that if we created a Subscriber Access number or enabled a user for voicemail it would fail with the following message:

> Attempts to route to servers in an Exchange UM Dialplan failed
> 
> No server in the dialplan [Hosted\_\_exap.um.outlook.com\_\_domain.onmicrosoft.com] accepted the call with id [7444acb26faf47a6bc12ea14e8b8727e].
> 
> Cause: Dialplan is not configured properly.
> 
> Resolution:
> 
> Check the configuration of the dialplan on Exchange UM Servers.

&nbsp;

When we would complete a trace on the SfB Edge Servers we found no errors except a 503 service unavailable coming from the O365 Servers.

Next we ran a series of Wireshark traces on the edge servers to see if there was anything happening and reviewed the firewall logs to see if some packets were getting dropped.  We knew the service was working as we had another user assigned to a different hosted voicemail policy and it worked without any issues.  In the firewall log files we found traffic from Office 365 failing on port 100.

**Solution**: A review of external DNS showed that a record was typo&#8217;ed into the system.  The SRV record was:

\_sip.\_tls.domain.com, Port 100, Weight 443, Priority 1

we corrected the DNS record to:

\_sip.\_tls.domain.com, Port 443, Weight 100, Priority 1

And the calls for Exchange SA and Voice Mail immediately started working.  I still can&#8217;t explain why the AA worked.

&nbsp;