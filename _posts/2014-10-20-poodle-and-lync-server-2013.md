---
id: 866
title: POODLE and Lync Server 2013
date: 2014-10-20T22:21:06+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=866
permalink: /2014/10/20/poodle-and-lync-server-2013/
st_layout_box:
  - right
panels_data:
  - 'a:0:{}'
categories:
  - Lync Server 2013
---
[UPDATE: 10/30/2014 &#8211; Microsoft has issued a statement that they are disabling SSL 3.0 in Office 365.]

[UPDATE: 11/20/2014 &#8211; Here are the [test results](http://masteringlync.com/2014/11/20/poodle-testing-results-for-lync/).]

Starting on December 1, 2014, Office 365 will begin disabling support for SSL 3.0. This means that from December 1, 2014, all client/browser combinations will need to utilize TLS 1.0 or higher to connect to Office 365 services without issues. This may require certain client/browser combinations to be updated.

<http://blogs.office.com/2014/10/29/protecting-ssl-3-0-vulnerability/>]

Let me start by saying I&#8217;m not a crypto genius.  This post is based on what I&#8217;ve been able to figure out based on my own research, talking to others and shouldn&#8217;t be considered the gospel.  If/when Microsoft releases an official statement, I&#8217;ll update this, eat crow, whatever it takes.  If you have any insight, feel free to drop me a note at <a href="mailto:richard@masteringlync.com" target="_blank">richard@masteringlync.com</a> or leave it in the comments and I&#8217;ll add your words to this post.  Let&#8217;s call this a community post.

After a conversation with another engineer this afternoon I decided I needed to take some time to learn SSL vs TLS vs what is going on.

**Overview**

SSL and TLS are not same thing.  TLS replaced SSL.  For a good overview of the two feel free to check out this <a href="http://en.wikipedia.org/wiki/Transport_Layer_Security" target="_blank">Wikipedia </a>article.  So from old to new, your versions are:

SSL2, SSL3, TLS 1.0 (sometimes referred to as SSL3.1), TLS 1.1, TLS 1.2.  TLS 1.3 is in draft but not yet available.

SSL 3 came out back in 1996.  I honestly had no clue it was that old.

**Why Do We Care**

Google announced four days ago a security vulnerability called POODLE which allows both a man in the middle attack by taking advantage of TLS Failback.  Essentially, every modern browser and server supports TLS 1.2 and would prefer that by default.  But just about every device still has SSL 3.0 enabled.  So the man in the middle attack basically makes the client unable to use TLS 1.2, TLS 1.1 or TLS 1.0 and go back to SSL 3.0.  Once there, apparently it easy to crack SSL 3.0.  According to this article (<https://www.openssl.org/~bodo/ssl-poodle.pdf>) it can take on average 256 requests using the BEAST attack to decrypt one byte of encrypted information.  Rinse and repeat you have the encrypted content.  I think sometimes being a crypto guy would be fun!

**What has Microsoft said?**

Thus far, not too much.  At this time, the only item I&#8217;ve found is this article:

<a href="https://technet.microsoft.com/library/security/3009008" target="_parent">https://technet.microsoft.com/library/security/3009008</a>

It recommends to disable SSL 3.0 but it doesn&#8217;t call out Lync or Exchange.

**What To Do?**

Ideally we would just disable SSL 3.0 and never have the issue but it&#8217;s not that simple.  Based on everything I&#8217;ve read, Windows XP doesn’t support anything past SSL 3.0 &#8211; at least in the browser level.  It appears once IE7 was installed TLS 1.0 exists at the OS level.

So can we disable SSL 3.0 in Lync?  Based on my testing, I would say yes.  In my lab as a test, I have disabled SSL 3.0 on my Edge, Front-End, Reverse Proxy (IIS/AAR) and OWAS server and ran through a laundry list of tests and it works fine.  Additionally, Lync Server 2013 supports FIPS compliance (<http://technet.microsoft.com/en-us/library/gg398577.aspx>) and SSL 3.0 must be disabled for FIPS compliance so I think that is a tacit consent of support for disabling SSL 3.0.  As of this post, I haven&#8217;t tested Aries or other phone vendors to make sure those work as expected.

So let’s say we can do this.  Here are a few things you would need to be aware of.

Windows 2008 R2 supports TLS 1.1 and TLS 1.2 but it wasn&#8217;t enabled by default.  Below are the scripts to enable TLS 1.0, TLS 1.1 and TLS 1.2 can be found here:

[UPDATE: This information taken from <a href="http://www.derekseaman.com/2010/06/enable-tls-12-aes-256-and-sha-256-in.html" target="_blank">here </a>and <a href="http://www.adminhorror.com/2011/10/enable-tls-11-and-tls-12-on-windows_1853.html" target="_blank">here</a>.  Looking for an official document of what was enabled by default in 2008 R2 still.]

[EnableTLS](http://masteringlync.com/wp-content/uploads/sites/2/2014/10/EnableTLS.txt)

Windows Server 2012 and Windows Server 2012 R2 have TLS 1.0 and 1.2 enabled by default, along with SSL 3.0.

To disable SSL 3.0 and 2.0  you can use these:

_\# Disable SSL 3.0 (PCI Compliance) and enable “Poodle” protection_

_md ‘HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server’ -Force_

_New-ItemProperty -path ‘HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server’ -name Enabled -value 0 -PropertyType ‘DWord’ -Force_

_\# Disable SSL 2.0 (PCI Compliance)_

_md ‘HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server’ -Force_

_New-ItemProperty -path ‘HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server’ -name Enabled -value 0 -PropertyType ‘DWord’ -Force_

&nbsp;

You will need to reboot the server when completed.

&nbsp;

If you want to test your server and make sure SSL 3.0 is actually disabled, you can use this site:

<https://www.ssllabs.com/ssltest/>

&nbsp;

That test only works if your server is on Port 443.  If you have a single IP Edge server, you will need to use OpenSSL to test.

openssl.exe s_client -connect sip.contoso.com:5061 -ssl3

openssl.exe s_client -connect sip.contoso.com:5061 –ssl2

I hope that helps.  Again, if you have more to add, please let me know via e-mail or comments.

&nbsp;

Scripts for Enabled/Disabling SSL in Windows comes from <a href="http://www.hass.de/content/setup-your-iis-ssl-perfect-forward-secrecy-and-tls-12" target="_blank">here</a>.  This script also changes the crypto order to enable Perfect Forward Secrecy.  I don&#8217;t know if Lync cares about that part or not.  More testing needs to happen.