---
id: 1382
title: 'Quick Tip: iOS 6.14.0.224 Breaks Login in Some Scenarios'
date: 2017-04-26T17:47:51+00:00
author: Richard Brynteson
layout: post
guid: http://masteringlync.com/?p=1382
permalink: /2017/04/26/quick-tip-ios-6-14-0-224-breaks-login-in-some-scenarios/
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
Fast patching and lack of testing results in bad things.

**Scenario:**

With the recent release of iOS 6.14.0.224 client we have found that many customers are having all sorts of problems being able to login.  This problem only occurs if a customer uses an HTTP to HTTP redirect.  This is a very common scenario when customers have lots of SIP Domains and doesn&#8217;t want to purchase lyncdiscover.domain.com for every domain (because if you have 100 SIP domains that certificate is really expensive!).

So a common practice is to open port 80 (HTTP) and have the initial client request happen to lyncdiscover.domain.com.  The actual request that goes out includes the users SIP Address at the end of the request.  At this point in time, no password or anything else has been sent out, the server sees this message request on 80 and tells the client to instead go to https://extwebservername.domain.com.  That domain may or may not be in the same domain.  This configuration can be found in this TechNet Article:

[https://technet.microsoft.com/en-us/library/mt723407.aspx#SupportedTopos](https://na01.safelinks.protection.outlook.com/?url=https%3A%2F%2Ftechnet.microsoft.com%2Fen-us%2Flibrary%2Fmt723407.aspx%23SupportedTopos&data=02%7C01%7Crigarcia%40microsoft.com%7C4cbe3384c5634f7f3ea508d48cbf8500%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C636288201599613172&sdata=9yxIZeZoP5mpyYMv0wDh3l6VhGw2L2cdfmSZAsUFhOM%3D&reserved=0)

&#8220;This should be a straightforward process if you&#8217;re requesting the new certs off an internal CA (certificate authority), but public certificates are more complex, and potentially a lot more expensive to re-request, not to mention it may be costly to add a lot of SIP domains to a new public cert. In that situation, there is an approach that&#8217;s supported, but **not recommended**. You can configure your reverse proxy to make the initial Autodiscover service request over port 80, which will use HTTP, rather than port 443, which is HTTPS (and 443 is the default configuration). That incoming request will be redirected to port 8080 on your Front End pool or Director. By doing this, you won&#8217;t need to make any certificate changes, because this traffic isn&#8217;t using HTTPS for requests. But again, we don&#8217;t recommend this, although it will work for you.&#8221;

Although it&#8217;s listed as &#8220;not recommended&#8221; there is no good reason to not do this.  Even if you have lyncdiscover.domain.com on your SSL Certificate the auth process is exactly the same.  The client sends it&#8217;s SIP Address in the request and the user is redirected to the same https://extwebservername.domain.com address.  Essentially, you are potentially spending 10 of thousands of dollars on a redirect URL.

The latest iOS client simply doesn&#8217;t understand this redirect process.  So anyone updating their client will see a failure.  No good at all.

**Solution:**

UPDATE 4/28/2017 &#8211; The fix is out.  Build 6.14.1.229 of the iOS Client will allow you to login again.

&nbsp;

Don&#8217;t update your client.  There isn&#8217;t a fix at this time.  We have an open ticket with Microsoft as they work on reproducing the issue.