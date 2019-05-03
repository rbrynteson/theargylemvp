---
id: 140
title: Using IIS Application Request Routing (ARR) as a TMG Replacement
date: 2013-02-12T06:00:21+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=140
permalink: /2013/02/12/using-iis-application-request-routing-arr-as-a-tmg-replacement/
st_layout_box:
  - right
panels_data:
  - 'a:0:{}'
categories:
  - Lync Server 2013
tags:
  - IIS ARR
  - Lync Server 2010
  - Lync Server 2013
  - TMG
---
So this won&#8217;t be shocking news but Microsoft has stopped selling Forefront Threat Management Gateway (TMG) and they really didn&#8217;t give us any good alternatives.  Officially, they tell you to use the Unified Access Gateway but anyone who uses it knows that 1) it&#8217;s a massive pain to setup 2) it&#8217;s really expensive 3) it breaks autodiscover and mobility.

So this leaves us to use third party hardware load balancers but I&#8217;m not much of a fan of doing firewall rules directly to the HLB.  The nice thing about the reverse proxy is that it served as a separation between the internet and production.  So I have found some articles on the web about IIS ARR with Lync but they all seem to be 1) not written for Lync 2013 2) don&#8217;t work with mobility 3) are too vague 4) assume that you are going to bind one service to one IP &#8211; which works find but sometimes people want to not take up a ton of IP&#8217;s on the internet to deploy one service.

So this will attempt to guide you through the configuration of ARR to work with Lync Server 2013.

UPDATE: I&#8217;ve been asked does this work with Lync Server 2010 and the answer is yes.  I haven&#8217;t spent as much time testing every option yet but it does appear to work fine.

**Installation**

First you need to install IIS on your Windows 2008 R2 or Windows 2012 Server.  I like to click the Application Server which will install pretty much everything you need in terms of .NET Framework.

The second item you need is ARR. This can be installed by downloading version 2.5 + hotfixes from Microsoft&#8217;s website or you can use the Web Installer (<http://www.microsoft.com/web/downloads/platform.aspx>).  This is how I typically install the product:

<a href="http://masteringlync.com/2013/02/12/using-iis-application-request-routing-arr-as-a-tmg-replacement/pic1-2/" rel="attachment wp-att-141"><img class="alignnone wp-image-141 size-medium" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/02/pic1-300x186.png?resize=300%2C186&#038;ssl=1" alt="pic1" width="300" height="186" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/02/pic1.png?resize=300%2C186&ssl=1 300w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/02/pic1.png?resize=768%2C477&ssl=1 768w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/02/pic1.png?w=904&ssl=1 904w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" /></a>

&nbsp;

Search for ARR and select Application Request Routing 2.5 (which will select the hotfixes) and install search for rewrite and install URL Rewrite 2.0.

NOTE: I see that 3.0 Beta is now available.  Feel free to try it out &#8211; it worked for me but I&#8217;m going to production with ARR so I want to use non-beta software for now.

**IIS Configuration**

This isn&#8217;t meant to be an all encompassing lesson on how to use IIS so I&#8217;m going to assume you have some basics under your belt.  You need to make sure that your public certificate is installed on the IIS server.  Go to bindings of your default website and bind your SSL certificate to this server.  This certificate must have a private key associated with it.  Also, make sure that your internal root certificate authority is installed on this server as well.  My test server is not domain joined so I have to install it manually.

Ultimately, your website is going to contain the IP address you plan to listen on.  So this is very similar to the concept of a listener from the TMG world.  You have the same limitation in terms of certificates and IP&#8217;s in IIS as you do in TMG.  So if you have two certificates you plan to use on the reverse proxy you will need to bind two IP&#8217;s to your IIS server and create two websites.  This guide is going to assume a single website/single IP address for Lync, Exchange and Office Web Apps (OWAS) as I have a single certificate with all of those names on it.

**ARR Configuration**

NOTE: I had a section on how to enable the proxy within ARR here at first because I was under the impression it was needed for this to work.  Got word from a Lync friend that it wasn&#8217;t needed and indeed it was an extra step in the configuration that didn&#8217;t harm anything just wasn&#8217;t required for the configuration.  Thank Corrado!

**Server Farms**

Server farms are the targets of where we might send traffic to.  There isn&#8217;t a direct correlation to something in TMG but the big thing to know is that ARR does more than just a reverse proxy.  It is a load balancer and more.  So we need to create a farm for Lync.

In IIS Manager, Click on **Server Farms** and right-click and choose **Create Server Farm**.

<a href="http://masteringlync.com/2013/02/12/using-iis-application-request-routing-arr-as-a-tmg-replacement/pic5-2/" rel="attachment wp-att-145"><img class="alignnone wp-image-145 size-full" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/02/pic5.png?resize=290%2C148&#038;ssl=1" alt="pic5" width="290" height="148" data-recalc-dims="1" /></a>

&nbsp;

Enter a name

<a href="http://masteringlync.com/2013/02/12/using-iis-application-request-routing-arr-as-a-tmg-replacement/pic6-2/" rel="attachment wp-att-146"><img class="alignnone wp-image-146 size-full" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/02/pic6.png?resize=578%2C440&#038;ssl=1" alt="pic6" width="578" height="440" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/02/pic6.png?w=578&ssl=1 578w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/02/pic6.png?resize=300%2C228&ssl=1 300w" sizes="(max-width: 578px) 100vw, 578px" data-recalc-dims="1" /></a>

Click Next

<a href="http://masteringlync.com/2013/02/12/using-iis-application-request-routing-arr-as-a-tmg-replacement/pic7/" rel="attachment wp-att-147"><img class="alignnone wp-image-147 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/02/pic7.png?resize=579%2C441&#038;ssl=1" alt="pic7" width="579" height="441" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/02/pic7.png?w=579&ssl=1 579w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/02/pic7.png?resize=300%2C228&ssl=1 300w" sizes="(max-width: 579px) 100vw, 579px" data-recalc-dims="1" /></a>

Enter the server IP address.  Click on advanced settings and change httpPort to 8080 and httpsPort to 4443.  If you are going to use the load balancing feature, add all of your front-end servers in the pool.  If you have a HLB, than this server IP should be going to your HLB.

<a href="http://masteringlync.com/2013/02/12/using-iis-application-request-routing-arr-as-a-tmg-replacement/pic8/" rel="attachment wp-att-148"><img class="alignnone wp-image-148 size-full" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/02/pic8.png?resize=415%2C162&#038;ssl=1" alt="pic8" width="415" height="162" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/02/pic8.png?w=415&ssl=1 415w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/02/pic8.png?resize=300%2C117&ssl=1 300w" sizes="(max-width: 415px) 100vw, 415px" data-recalc-dims="1" /></a>

&nbsp;

You will get this message.  Click the Yes button to create a default rules.

<a href="http://masteringlync.com/2013/02/12/using-iis-application-request-routing-arr-as-a-tmg-replacement/pic9/" rel="attachment wp-att-149"><img class="alignnone wp-image-149 size-full" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/02/pic9.png?resize=786%2C111&#038;ssl=1" alt="pic9" width="786" height="111" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/02/pic9.png?w=786&ssl=1 786w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/02/pic9.png?resize=300%2C42&ssl=1 300w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/02/pic9.png?resize=768%2C108&ssl=1 768w" sizes="(max-width: 786px) 100vw, 786px" data-recalc-dims="1" /></a>

Now you should see your new server farm.  Click on the Lync farm you just created and find **Routing Rules**.  Double click on it and disable **SSL Offloading**. There is no reason to offload on the ARR box as far as I can figure out.  If someone has a good reason to leave it on let me know.

<a href="http://masteringlync.com/2013/02/12/using-iis-application-request-routing-arr-as-a-tmg-replacement/pic10/" rel="attachment wp-att-150"><img class="alignnone wp-image-150 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/02/pic10.png?resize=373%2C283&#038;ssl=1" alt="pic10" width="373" height="283" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/02/pic10.png?w=373&ssl=1 373w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/02/pic10.png?resize=300%2C228&ssl=1 300w" sizes="(max-width: 373px) 100vw, 373px" data-recalc-dims="1" /></a>

Make sure to click **Apply** in the Action pane on the right when you are making changes in IIS.

Your farm is now configured and setup.  If you want you can create two more farms, one for Exchange and one for OWAS.

**URL Rewrite Rules**

Now it&#8217;s time to get to the meat and potatoes of the configuration.  We are now going to setup URL Rewrite (or really reroute rules).  This is very similar to the firewall rules that you had in TMG.  They don&#8217;t have all of the same features but covers what we need.

In IIS Manager, click on the Server

<a href="http://masteringlync.com/2013/02/12/using-iis-application-request-routing-arr-as-a-tmg-replacement/pic11/" rel="attachment wp-att-152"><img class="alignnone wp-image-152 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/02/pic11.png?resize=297%2C149&#038;ssl=1" alt="pic11" width="297" height="149" data-recalc-dims="1" /></a>

&nbsp;

**NOTE**: This is URL Rewrite on the server.  Don&#8217;t go to the website.  There is a URL rewrite option there as well.

When you go into URL Rewrite you will find two default rules created for you.  The first is the SSL rule and the second is the HTTP rule.  Since I don&#8217;t want to use the HTTP rules I am simply disabling them.  You could delete them if you want I suppose as well.

<a href="http://masteringlync.com/2013/02/12/using-iis-application-request-routing-arr-as-a-tmg-replacement/pic12/" rel="attachment wp-att-153"><img class="alignnone wp-image-153 size-full" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/02/pic12.png?resize=625%2C214&#038;ssl=1" alt="pic12" width="625" height="214" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/02/pic12.png?w=625&ssl=1 625w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/02/pic12.png?resize=300%2C103&ssl=1 300w" sizes="(max-width: 625px) 100vw, 625px" data-recalc-dims="1" /></a>

&nbsp;

Double click the ARR\_Lync\_Loadbalance_SSL rule and let&#8217;s understand what you see here:

<a href="http://masteringlync.com/2013/02/12/using-iis-application-request-routing-arr-as-a-tmg-replacement/pic13/" rel="attachment wp-att-154"><img class="alignnone wp-image-154 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/02/pic13.png?resize=591%2C437&#038;ssl=1" alt="pic13" width="591" height="437" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/02/pic13.png?w=591&ssl=1 591w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/02/pic13.png?resize=300%2C222&ssl=1 300w" sizes="(max-width: 591px) 100vw, 591px" data-recalc-dims="1" /></a>

&nbsp;

In the Match URL is basically what we are going to make after the / in the URL.  So if our URL was www.domain.com/website the pattern would be /website for example.  You will see under Using you will have the option to use Regular Expressions.

The Conditions is a set of inputs that are required to match this rule.  Here we will we have HTTPS which basically means we must match SSL requests only.

<a href="http://masteringlync.com/2013/02/12/using-iis-application-request-routing-arr-as-a-tmg-replacement/pic14/" rel="attachment wp-att-155"><img class="alignnone wp-image-155 size-full" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/02/pic14.png?resize=586%2C274&#038;ssl=1" alt="pic14" width="586" height="274" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/02/pic14.png?w=586&ssl=1 586w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/02/pic14.png?resize=300%2C140&ssl=1 300w" sizes="(max-width: 586px) 100vw, 586px" data-recalc-dims="1" /></a>

&nbsp;

This is a continuation of the above.  The Action section tells us what we should do if we match.  So here we will route to the Lync Server Farm.  This part is pretty straight forward and will be basically the same for all rules.  So let&#8217;s create our first Lync Rule.

**Meeting/Dialin/External Web Services Rule**

Here you have some options based on how you do your simple URLs.  If you do &#8220;Option A&#8221; where you URL would look like:

https://meet.domain.com and https://dialin.domain.com then you would need to use the below rule.

<a href="http://masteringlync.com/2013/02/12/using-iis-application-request-routing-arr-as-a-tmg-replacement/pic1-3/" rel="attachment wp-att-168"><img class="alignnone wp-image-168 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/02/pic19.png?resize=598%2C390&#038;ssl=1" alt="pic1" width="598" height="390" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/02/pic19.png?w=598&ssl=1 598w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/02/pic19.png?resize=300%2C196&ssl=1 300w" sizes="(max-width: 598px) 100vw, 598px" data-recalc-dims="1" /></a>

Change Using to: Regular Expression

Change Pattern to: (.*)

If you use Option B where your simple URLs might look like this:

https://meet.domain.com/ID/ and https://meet.domain.com/dialin/ your rule would look like this.

<a href="http://masteringlync.com/2013/02/12/using-iis-application-request-routing-arr-as-a-tmg-replacement/pic2-3/" rel="attachment wp-att-169"><img class="alignnone wp-image-169 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/02/pic21.png?resize=595%2C440&#038;ssl=1" alt="pic2" width="595" height="440" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/02/pic21.png?w=595&ssl=1 595w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/02/pic21.png?resize=300%2C222&ssl=1 300w" sizes="(max-width: 595px) 100vw, 595px" data-recalc-dims="1" /></a>

((?:^dialin|^id|^Abs|^autodiscover|^CertProv|^CollabContent|^Fonts|^GroupExpansion|^HybridConfig|^lwa|^mcx|^PassiveAuth|^PersistentChat|^Reach|^RequestHandlerExt|^RgsClients|^Scheduler|^Storage|^ucwa|^WebTicket).*)

**NOTE:** You will need to change dialin and id to whatever you use if using Option B for simple URL&#8217;s

**NOTE**: This is the list of all potential directories as of RTM of Lync 2013.  CU1 will most likely add some items so this may need to be edited.

Add to Conditions: {HTTP_HOST} on the pattern of (externalwebservices.domain.com|dialin.domain.com|meet.domain.com).

**NOTE**: You should NOT add LyncDiscover.domain.com to this rule.  The reason we don&#8217;t want to add this is because our regular expression doesn&#8217;t include the root of the website and lyncdiscover.domain.com/?sipuri= won&#8217;t match any of these rules.  If you wanted to, you could change your pattern to (.*) and add lyncdiscover.domain.com to the list. I don&#8217;t have a good reason to do it either way.  Whatever makes the most sense to you but I like to separate them out.

**Lync Discover Rule**

Now we create a rule for Lync Discover services.  Here is what I have created.  This rule is created as brand new and not one of the existing rules.

<a href="http://masteringlync.com/2013/02/12/using-iis-application-request-routing-arr-as-a-tmg-replacement/pic16/" rel="attachment wp-att-157"><img class="alignnone wp-image-157 size-full" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/02/pic16.png?resize=590%2C438&#038;ssl=1" alt="pic16" width="590" height="438" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/02/pic16.png?w=590&ssl=1 590w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/02/pic16.png?resize=300%2C223&ssl=1 300w" sizes="(max-width: 590px) 100vw, 590px" data-recalc-dims="1" /></a>

&nbsp;

Here I am defining my pattern match as any request (.*) anything from lyncdiscover.thegaragelab.info URL.  Here I am not requiring HTTPS on this rule Lync Discover will use either HTTP or HTTPS.

**OWAS Rule Setup**

If you created your web server farm for OWAS you should have your default rules again.  So if I look at that rule:

<a href="http://masteringlync.com/2013/02/12/using-iis-application-request-routing-arr-as-a-tmg-replacement/pic17/" rel="attachment wp-att-158"><img class="alignnone wp-image-158 size-full" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/02/pic17.png?resize=591%2C428&#038;ssl=1" alt="pic17" width="591" height="428" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/02/pic17.png?w=591&ssl=1 591w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/02/pic17.png?resize=300%2C217&ssl=1 300w" sizes="(max-width: 591px) 100vw, 591px" data-recalc-dims="1" /></a>

&nbsp;

This rule again is pretty straight forward.  We are matching anything intended to go to owas.domain.com.  The big difference is under Action, our Server Farm is now OWAS and not Lync.  You can do the same thing with Exchange or other services as well.  Here is my completed configuration:

<a href="http://masteringlync.com/2013/02/12/using-iis-application-request-routing-arr-as-a-tmg-replacement/pic18/" rel="attachment wp-att-159"><img class="alignnone wp-image-159 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/02/pic18.png?resize=712%2C162&#038;ssl=1" alt="pic18" width="712" height="162" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/02/pic18.png?w=712&ssl=1 712w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/02/pic18.png?resize=300%2C68&ssl=1 300w" sizes="(max-width: 712px) 100vw, 712px" data-recalc-dims="1" /></a>

&nbsp;

**Services Tested**

I have tested the following services to make sure they work through ARR.

  * Meeting Join URL
  * Dial-In Website
  * Web Scheduler
  * Mobility

The only thing I haven&#8217;t tested thus far is the Lync MX (Metro/Win 8) client.  I&#8217;ll try to get remote of my lab and test that later this week.  My history thus far with the Lync MX client is a complete crap shoot &#8211; where mobility works but MX doesn&#8217;t.

**Troubleshooting**

So what are some common problems I&#8217;ve found while setting up ARR.

#1 &#8211; Make sure you can reach your websites from your ARR server.  If the ARR server can&#8217;t reach these websites it&#8217;s not going to work from the outside.

#2 &#8211; Certificates.  Ensure that your internal server has your internal CA root certificate.  If this box isn&#8217;t a domain joined server, it won&#8217;t be there.  If you did #1 you shouldn&#8217;t have this issue because you would have fixed it when the certificate error appeared.

#3 &#8211; Make sure your internal certificates have the right names.  For example on OWAS.  If your internal FQDN is owas.intdomain.com and your external FQDN is owas.domain.com, your internal certificate should have BOTH names on it.  Your internal users will look up based on the internal name and when your external people go to that server, they will proxy through ARR with the external name.  It doesn&#8217;t have to be a public certificate but it needs to have the name.

Hopefully that covers the most issues you might run into and this gets you on the right page for using ARR and Lync 2013.

**UPDATE**

I found that you will want to adjust the timeout on the IIS or you will see some timeouts with the new Lync 2013 Mobile Client.  On the Proxy Settings | Time-out &#8211; change from 30 seconds to 180 second.

&nbsp;

<a href="http://masteringlync.com/2013/02/12/using-iis-application-request-routing-arr-as-a-tmg-replacement/picture/" rel="attachment wp-att-199"><img class="alignnone wp-image-199 size-medium" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/02/Picture-278x300.jpg?resize=278%2C300&#038;ssl=1" alt="Picture" width="278" height="300" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/02/Picture.jpg?resize=278%2C300&ssl=1 278w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/02/Picture.jpg?resize=768%2C829&ssl=1 768w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/02/Picture.jpg?w=814&ssl=1 814w" sizes="(max-width: 278px) 100vw, 278px" data-recalc-dims="1" /></a>

&nbsp;