---
id: 785
title: Using TCP/UDP Port Checker Web Service
date: 2014-07-15T03:06:16+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=785
permalink: /2014/07/15/using-tcpudp-port-checker-web-service/
st_layout_box:
  - right
panels_data:
  - 'a:0:{}'
categories:
  - Lync Server 2010
  - Lync Server 2013
---
As part of the newest <a href="http://lyncvalidator.com" target="_blank">LyncValidator </a>update is the addition of a web service for the purposes of checking availability of ports.  The following post details the use of the Web Service.

_License_

The TCP/UDP Port Check Web Service is provided as is.  We don&#8217;t make any promises to services availability although the service is hosted on Windows Azure so it should be pretty stable.

The Web Service will be offered as both a no cost and cost options.  To use the public Web Service, there is no cost, all that is required is the following:

1. Register by e-mailing me at <a href="mailto:richard@masteringlync.com" target="_blank">richard@masteringlync.com</a>.  All I need to know is: A) How do you plan on using it.  B) Estimated requests daily/monthly.

2. Display acknowledgement of _Port Validation by Richard Brynteson, MasteringLync.com/LyncValidator.com_ on your application, web page, generation of reports, etc.

If you don&#8217;t want to display anything you should e-mail me about licensing costs.  I reserve the right to change these terms whenever I want.

_How it Works_

There are two different available checks.  The first is a TCP check done by doing a simple TCP Port Query to ensure the service is available and responding.  The second check is a UDP check.  This is done by doing an allocation to the Lync Server AV Edge Service.

_Usage_

The service is available as a standard web service.  You can view the WSDL by visiting here:

<http://gcmsdk.azurewebsites.net/validator.asmx?wsdl>

and the specific tool is here:

<http://gcmsdk.azurewebsites.net/validator.asmx?op=portChecker>

To use within a Web or Windows Application, you can do the following:

1. Add a Web Service Reference to your project.

2. Add a reference to the page/project (i.e. using WebValidator;)

3. Pass the four required attributes to the web service.  You need to pass the IP or FQDN, Port, Protocol (UDP or TCP) and your license key.  Here is an example:

WebValidator.<span style="color: #2b91af;font-family: Consolas;font-size: small"><span style="color: #2b91af;font-family: Consolas;font-size: small"><span style="color: #2b91af;font-family: Consolas;font-size: small">Validator</span></span></span><span style="font-family: Consolas;font-size: small"><span style="font-family: Consolas;font-size: small"> web = </span></span><span style="color: #0000ff;font-family: Consolas;font-size: small"><span style="color: #0000ff;font-family: Consolas;font-size: small"><span style="color: #0000ff;font-family: Consolas;font-size: small">new </span></span></span><span style="color: #2b91af;font-family: Consolas;font-size: small"><span style="color: #2b91af;font-family: Consolas;font-size: small"><span style="color: #2b91af;font-family: Consolas;font-size: small">Validator</span></span></span><span style="font-family: Consolas;font-size: small"><span style="font-family: Consolas;font-size: small">();</span></span>

<span style="color: #0000ff;font-family: Consolas;font-size: small"><span style="color: #0000ff;font-family: Consolas;font-size: small"><span style="color: #0000ff;font-family: Consolas;font-size: small">string</span></span></span><span style="font-family: Consolas;font-size: small"><span style="font-family: Consolas;font-size: small"> Result= web.portChecker(IPorFQDN, Port, UDP</span></span><span style="font-family: Consolas;font-size: small"><span style="font-family: Consolas;font-size: small">, </span></span><span style="color: #a31515;font-family: Consolas;font-size: small"><span style="color: #a31515;font-family: Consolas;font-size: small"><span style="color: #a31515;font-family: Consolas;font-size: small">&#8220;xxxxx-xxxxx-xxxxx&#8221;</span></span></span><span style="font-family: Consolas;font-size: small"><span style="font-family: Consolas;font-size: small">);</span></span>

This will return either a string value of Open or CLOSED.  For example:

[<img class="alignnone wp-image-787 size-full" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/07/pic12.png?resize=175%2C47&#038;ssl=1" alt="pic1" width="175" height="47" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.com/files/2014/07/pic12.png)

And that is all it takes.  The key you will receive as part of registration.  If you have any questions just ask via Twitter or e-mail.