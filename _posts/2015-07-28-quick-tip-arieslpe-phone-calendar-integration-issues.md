---
id: 1178
title: 'Quick Tip: Aries/LPE Phone / Calendar Integration Issues'
date: 2015-07-28T15:39:41+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=1178
permalink: /2015/07/28/quick-tip-arieslpe-phone-calendar-integration-issues/
panels_data:
  - 'a:0:{}'
categories:
  - Exchange 2013
  - Exchange Online
  - Lync Server 2013
  - Skype for Business
---
**Problem**: Although 3PIP phones continue to improve with each release we find that many companies are still going back to the CX series phones because they are built well and simply work.  Recently, I ran into an Exchange integration error that was different then the well documents yellow triangle of death (<a href="http://blog.schertz.name/2012/03/troubleshooting-lync-phone-edition-issues/" target="_blank">Jeff&#8217;s Post</a>, <a href="http://ucken.blogspot.com/2011/09/lync-deskphones-and-exchange.html" target="_blank">Ken&#8217;s Post</a>).  The environment wasn&#8217;t anything overly complex &#8211; SfB 2015 with Exchange Online (Office 365).

In my scenario, the phone would simply hang on &#8220;Connecting to Exchange Server&#8221; and never complete.  So I wouldn&#8217;t get far enough to even get the yellow triangle of death.

I knew the users Proxy Address/SMTP address was set correct as the Lync/SfB Client itself had no problem displaying the Exchange Calendar information.  So I started by using Andrew Morpeth&#8217;s awesome <a href="https://gallery.technet.microsoft.com/lync/Lync-Phone-Edition-LPE-Log-e1686e46" target="_blank">LPE Log Viewer</a> to see if there was anything in the logs that would point me in the right direction.  Looking through the logs the only thing I found that was remotely interesting was this error:

> Failed to find smtp email address from local contact model.  Waiting for contact card event from the local contact model.

That seemed odd, why was the phone looking for the users e-mail address from the contact card and not the UPN or Proxy Address.  So I decided it was time to go back to Active Directory and see if there was anything missing from the user.  When viewing the AD Attributes from a working user to a non-working user I found that the mail attribute for the non-working user was blank.  For some reason, the provisioning process missed assigning it for this user.

So I went and added the mail attribute to the account and after about 15 minutes the phone calendar integration started working.  No reboot was even necessary.

**Solution**: This was good for a single user fix but it appeared as though we have a few hundred users that were setup incorrectly in this environment.  Since the UPN and E-Mail addresses matched for all of the users the fix was simple:

> Get-ADUser -Filter * -SearchBase &#8220;ou=usersou,dc=contoso,dc=com&#8221; | Foreach-Object {  
> Set-ADUser -Identity $_ -Email &#8220;$($_.userPrincipalName)&#8221; -Verbose  
> }

The moral of the story is this.  The Lync/SfB Client determines the e-mail address based on the users UPN and/or the Proxy/SMTP Address defined in Active Directory.  Aries/LPE Phones determine the e-mail address solely off the mail attribute in Active Directory.

&nbsp;