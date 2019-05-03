---
id: 906
title: 'Quick Tip: The buffer supplied to a function was too small'
date: 2014-12-09T14:59:12+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=906
permalink: /2014/12/09/quick-tip-the-buffer-supplied-to-a-function-was-too-small/
st_layout_box:
  - right
panels_data:
  - 'a:0:{}'
categories:
  - Lync Server 2010
  - Lync Server 2013
---
With all of the improvements that has happened within the deployment process for Lync Server 2010/2013 interestingly certificates is still listed as the #1 issue but personally I almost never see those issues.  Well, today&#8217;s quick tip is one of those certificate issues that I had not yet seen but certainly isn&#8217;t &#8220;normal&#8221;.

**Someone CNG&#8217;ed my Cert**

The issue was clear.  Upon trying to assign the certificate to my front-end pool I was met with this error:

[<img class="alignnone wp-image-907 size-medium" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/12/Untitled-300x226.png?resize=300%2C226&#038;ssl=1" alt="" width="300" height="226" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/12/Untitled.png?resize=300%2C226&ssl=1 300w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/12/Untitled.png?w=610&ssl=1 610w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/12/Untitled.png)

A quick search told me the issue was certificate related.  At first I thought the issue was with the fact that this certificate didn&#8217;t match all of the names Lync thought should be on the certificate (there is a SIP domain that isn&#8217;t going to be used but of course lyncdiscover.thatdomain.com shows up on the desired cert list).  So after verifying that wasn&#8217;t our problem I started hunting around.

I remembered this [blog post](http://www.ehloworld.com/751) from Pat Richard about being unable to download the address book.  I won&#8217;t rehash the entire post, you should read it, but essentially the issue is that newer CA&#8217;s issue certs with CNG or <a title="Cryptography Next Generation" href="http://technet.microsoft.com/en-us/library/cc730763(WS.10).aspx" target="_blank">Cryptography Next Generation</a>.  His looks like it&#8217;s a Lync 2010 deployment so maybe that is why he was able to apply this cert and I wasn&#8217;t.  Not sure on that one.  Pat details that if you run this command:

_certutil -v -store my &#8220;18 3b 41 c3 00 01 00 00 1f 9b&#8221;_

Where &#8220;18 3b 41 c3 00 01 00 00 1f 9b&#8221; is the serial number of your certificate that you will get back the detailed information about the certificate.  It will dump a bunch to the screen but you are looking for:<figure id="attachment_908" style="width: 646px" class="wp-caption alignnone">

[<img class="wp-image-908 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/12/pic2.png?resize=646%2C114&#038;ssl=1" alt="" width="646" height="114" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/12/pic2.png?w=646&ssl=1 646w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/12/pic2.png?resize=300%2C53&ssl=1 300w" sizes="(max-width: 646px) 100vw, 646px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/12/pic2.png)<figcaption class="wp-caption-text">Good Cert</figcaption></figure> 

&nbsp;<figure id="attachment_912" style="width: 642px" class="wp-caption alignnone">

[<img class="wp-image-912 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/12/Untitled1.png?resize=642%2C113&#038;ssl=1" alt="" width="642" height="113" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/12/Untitled1.png?w=642&ssl=1 642w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/12/Untitled1.png?resize=300%2C53&ssl=1 300w" sizes="(max-width: 642px) 100vw, 642px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/12/Untitled1.png)<figcaption class="wp-caption-text">Bad Cert &#8211; Notice the KeySpec is blank.</figcaption></figure> 

&nbsp;

&nbsp;

The last line is the one you care about, KeySpec.  If the certificate has CNG this field will be blank is will fail when attempting to assign to Lync Services.  If the cert doesn&#8217;t have CNG (and therefore is usable) it will have KeySpec = 1 &#8211;AT_KEYEXCHANGE as a value.

**Change That Cert**

So the issue has been found.  The solution could be easy.  Simply reissue the certificate.  The problem here is in some organizations a reissue of a certificate can take a VERY LONG TIME!  So my goal was to change the certificate without having to reissue.  This is where <a href="http://slproweb.com/products/Win32OpenSSL.html" target="_blank">OpenSSL </a>comes in.

These steps will essentially remove the CNG (crypto folks will hate I just made it sound so trivia).

1. Download and install OpenSSL to your computer.  It&#8217;s a powerful package so you should have it anyways.

2. Export your certificate with private key.  This can be done via the MMC | Certificates.  If you didn&#8217;t mark your cert as exportable when you issued it you have committed the first sin of Lync Deployments.  Always mark your certs as exportable.

3. Convert the cert to PEM.  Open up CMD and browse to the OpenSSL install directory.

openssl.exe pkcs12 -in lynccert.pfx -out lynccert.pem -nodes

It will prompt you for your private key password.

4. Convert the PEM back to PFX.

openssl.exe pkcs12 -export -in lynccert.pem -out lynccert\_no\_cng.pfx

It will again ask for a password in this process.

At this point in time, you can go back to the MMC, delete the old certificate with CNG and import in your newly &#8220;converted&#8221; certificate.  Hope that helps someone else along the way.

&nbsp;