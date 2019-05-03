---
id: 821
title: Reverse DNS/PTR Record for Microsoft Azure
date: 2014-09-09T14:30:47+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=821
permalink: /2014/09/09/reverse-dnsptr-record-for-microsoft-azure/
st_layout_box:
  - right
panels_data:
  - 'a:0:{}'
categories:
  - Azure
---
We are going to take a step out of the Lync Department for a post (and add a new category on the blog) and chat about DNS/Reverse DNS/PTR records in Microsoft Azure.

**Scenario**

Windows Virtual Machine running Microsoft Exchange (or another e-mail product) you want to ensure that when sending e-mail you don&#8217;t get bounce backs due to reverse DNS lookup issue.  I found my process through what felt like several hundred Bing searches (and that other search engine also) so I thought I would post my entire process in a single location.  If for nothing else, so I remember what the process is next time I need to configure and set this up.

**DNS Configuration**

The first thing I need to do is get the public IP Address of my virtual machine.  (NOTE: As far as I can tell, as long as I never shutdown my virtual machine, it will keep the public/private IP assigned to it forever.)  Logging into the Azure Portal and navigating to Virtual Machines you should find the public/private IP.

[<img class="alignnone wp-image-822 size-full" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/09/pic1.png?resize=203%2C170&#038;ssl=1" alt="pic1" width="203" height="170" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.com/files/2014/09/pic1.png)

My domain is registered through GoDaddy and as such I&#8217;ve been using their DNS Manager for all record keeping.  Next, we go to the GoDaddy DNS Manager and set the following items:

<table>
  <tr>
    <td>
      Type
    </td>
    
    <td>
      Host
    </td>
    
    <td>
      Points To
    </td>
  </tr>
  
  <tr>
    <td>
      A
    </td>
    
    <td>
      @
    </td>
    
    <td>
      Public IP From Azure
    </td>
  </tr>
  
  <tr>
    <td>
      CNAME
    </td>
    
    <td>
      www
    </td>
    
    <td>
      @
    </td>
  </tr>
  
  <tr>
    <td>
      CNAME
    </td>
    
    <td>
      mail
    </td>
    
    <td>
      service.cloudapp.net
    </td>
  </tr>
  
  <tr>
    <td>
      MX
    </td>
    
    <td>
      @
    </td>
    
    <td>
      mail.domain.com
    </td>
  </tr>
  
  <tr>
    <td>
      TXT
    </td>
    
    <td>
      mail.domain.com
    </td>
    
    <td>
      v=spf1 a mx ptr ~all
    </td>
  </tr>
</table>

&nbsp;

The resulting DNS should look a lot like this:

[<img class="alignnone wp-image-823 size-full" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/09/pic2.png?resize=701%2C428&#038;ssl=1" alt="pic2" width="701" height="428" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/09/pic2.png?w=701&ssl=1 701w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/09/pic2.png?resize=300%2C183&ssl=1 300w" sizes="(max-width: 701px) 100vw, 701px" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.com/files/2014/09/pic2.png)

The WWW is obviously optional and I have no good reason to set it other than I set WWW all the time.

At this point in time, we have covered configuration of DNS.  We have our MX record working and a PTR record in place as well.  What is missing however, is the reverse DNS lookup.

**Microsoft Azure Configuration**

If you are like me, and have never connected to Microsoft Azure via PowerShell the first go round is a bit confusing.  It gets even more complicated when you deal with the difference between an Azure AD and a Microsoft ID login to Azure.  So let&#8217;s step through the process.

1. Install Microsoft Azure PowerShell.  The easiest way to accomplish this is through the <a href="http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409" target="_blank">Web Platform Installer</a>.  You can use that link to download and install.

2. Login to Microsoft Azure.  There are two methods to login.  Using Azure AD and Certificate.  I thought that launching Azure PowerShell and connecting via the

<div class="codecolorer-container text default" style="overflow:auto;white-space:nowrap;width:435px;">
  <div class="text codecolorer">
    Add-AzureAccount
  </div>
</div>

PowerShell cmdlet would work. It prompts you for credentials and it will failed for me.  This is because I had numerous other IE windows opened and logged into the Azure Portal, Office 365 or something else outside this subscription.  Additionally, I&#8217;ve found that using a Microsoft ID alone isn&#8217;t enough.  You need to make sure that the user is also an Azure AD Administrator.  So I like to login with certificate instead.

To login via certificate do the following:

A. Login to the Azure Portal with your default browser.

B. Launch the Azure PowerShell and type:

<div class="codecolorer-container text default" style="overflow:auto;white-space:nowrap;width:435px;">
  <div class="text codecolorer">
    Get-AzurePublishSettingsFile
  </div>
</div>

This will open your default browser and download a certificate file.  You should save this to your local computer.

NOTE: If you have more than a single Azure Subscription, you will want to make sure that before you run the Get-AzurePublishSettingsFile that you switch to the Azure Subscription you want to manage.

C. Import Azure Certificate into PowerShell

<div class="codecolorer-container text default" style="overflow:auto;white-space:nowrap;width:435px;">
  <div class="text codecolorer">
    Import-AzurePublishSettingsFile C:certs<SubscriptionName>-credentials.publishsettings
  </div>
</div>

D. You can use the Get-AzureSubscription and Select-AzureSubscription to switch between subscriptions.  You no longer have any need for a password to get into your Azure Account.  Make sure to protect your certificate file.

3. Get your list of Azure Services

<div class="codecolorer-container text default" style="overflow:auto;white-space:nowrap;width:435px;">
  <div class="text codecolorer">
    Get-AzureService | fl ServiceName
  </div>
</div>

[<img class="alignnone wp-image-824 size-full" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/09/pic3.png?resize=434%2C116&#038;ssl=1" alt="pic3" width="434" height="116" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/09/pic3.png?w=434&ssl=1 434w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/09/pic3.png?resize=300%2C80&ssl=1 300w" sizes="(max-width: 434px) 100vw, 434px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.com/files/2014/09/pic3.png)

Here you can see I have two services, both of which are virtual machines.  One is a DC and the second is a mail server.

4. Verify you see the service you want and run this command:

<div class="codecolorer-container text default" style="overflow:auto;white-space:nowrap;width:435px;">
  <div class="text codecolorer">
    Set-AzureService –ServiceName "gcmrelay" –Description "Reverse DNS" –ReverseDnsFqdn "mail.golfclubmgmt.net."
  </div>
</div>

In this command, I am specifying the service name, giving it a description and setting the Reverse DNS settings.  It is important to note a few things about the Reverse DNS FQDN.  It can ONLY be a vanity name (i.e. my domain and not cloudapp.net) if you have a CNAME record pointing to the vanity name.  If you recall my DNS settings, mail.golfclubmgmt.net points to gcmrelay.cloudapp.net.  Lastly, your Reverse DNS record should have a trailing . in the name.  That isn&#8217;t a typo above.

5. Test your deployment.  After you have given DNS some time to replicate and do it&#8217;s thing you can check your PTR/Reverse DNS settings.  There are many different MX/PTR lookup tools available however I have to say I prefer the <a href="https://www.umn.edu/dirtools/blockcheck" target="_blank">University of MN&#8217;s page</a>.  It does a great job of checking all sorts of errors in a single request.

That is it.  Your e-mail server should pass any PTR/Reverse DNS requirements and you should be safe to send e-mail from Azure.

&nbsp;