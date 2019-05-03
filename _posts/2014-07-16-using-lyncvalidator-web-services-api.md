---
id: 793
title: Using LyncValidator Web Services API
date: 2014-07-16T17:38:36+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=793
permalink: /2014/07/16/using-lyncvalidator-web-services-api/
st_layout_box:
  - right
categories:
  - Lync Server 2010
  - Lync Server 2013
---
See my [previous article](http://masteringlync.com/2014/07/15/using-tcpudp-port-checker-web-service/) for more details on the web services in general.

After publishing my article on creating a Web Service for checking TCP/UDP ports for the Lync Validator, I asked myself, why stop at only those.  I could really consolidate my code if I turned all checks into a web service.  And now I have an updated Lync Validator Web Service API.

The Lync Validator Web Service API does the following checks:

  * TCP/UDP Port Checking
  * Certificate Validation
  * Office Web App Validation
  * DNS Validation

The following details what needs to be passed and what is returned in each case.

<span style="text-decoration: underline">TCP/UDP Port Checking</span>

portChecker(string IP, Int Port, string Protocol, string Key);

  * IP = IP Address or FQDN
  * Port = Port to be checked
  * Protocol = UDP or TCP (only valid options)
  * Key = Access key provided to you.  In xxxxx-xxxxx-xxxxx format.

Returns a single string of Open or CLOSED.

<span style="text-decoration: underline">Certificate Validation</span>

certificateParse(string FQDN, string Key)

  * FQDN = FQDN of the server you wish to parse
  * Key = Access key provided to you.  In xxxxx-xxxxx-xxxxx format.

Returns a string[] array.

  * string[0] = General messages, such as success, failure reason, etc.
  * string[1] = Days until certificate expires
  * string[2] = Subject Name of Certificate
  * string[3] = All SAN&#8217;s on Certificate.  Separated by a < br /> as I&#8217;m using it on the web.

<span style="text-decoration: underline">Office Web Apps Validation</span>

owasValidation(string XML, string Key)

  * XML = Hosting Discovery page (i.e. <https://owas.domain.com/hosting/discovery/>).
  * Key = Access key provided to you.  In xxxxx-xxxxx-xxxxx format.

Returns a string[] array.

  * string[0] = True or False
  * string[1] = If false, the reason for the failure.

<span style="text-decoration: underline">DNS Validation</span>

dnsChecker(string FQDN, string ExpectedIP, string Type, string SRVService, string SRVProtocol, string SRVDomain, string SRVExpected, string Key)

  * FQDN = FQDN to be checked (Used only for A Record checks)
  * ExpectedIP = Expected IP Address to be returned (Used only for A Record checks)
  * Type = A or SRV
  * SRVService = Service for SRV Check.  _sip for example.
  * SRVProtocol = Protocol for SRV Check. _tls for example.
  * SRVDomain = Domain for SRV Check.  domain.com for example.
  * SRVExpected = Equals the expected value from the SRV check in this format: Priority Weight Port Address.  For example: 0 0 443 sip.domain.com
  * Key = Access key provided to you.  In xxxxx-xxxxx-xxxxx format.

Returns a string[] array.

  * string[0] = True, False or Warning.  Warning only occurs in SRV check if weight, priority or port don&#8217;t match.
  * string[1] = Return value.  In an A record check, it will be the IP address.  In an SRV check, the SRV record.  Each finishes with a < br /> again since I was lazy.

Future updates will include additional checks/validations for Lync.  Additionally, I will remove the silly line breaks from the results in the future and simply return back the raw information.  If you think of any other validations I&#8217;m missing thus far and should be moved to the top of the list, let me know.  My guess is that I already have them on the list but you never know.