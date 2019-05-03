---
id: 1614
title: Validator SDK Update
date: 2018-09-13T12:06:04+00:00
author: Richard Brynteson
layout: post
guid: http://masteringlync.com/?p=1614
permalink: /2018/09/13/validator-sdk-update/
colormag_page_layout:
  - default_layout
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
image: https://masteringlync.com/wp-content/uploads/sites/2/2018/09/validation.jpg
categories:
  - Development
  - Featured
  - Skypevalidator
---
This has been a project that has taken WAY to long to complete not because it was difficult but because I was just busy.  But now it&#8217;s a priority to get it done as I need to consume <a href="https://SkypeValidator.com" target="_blank" rel="noopener">SkypeValidator.com</a> API&#8217;s in a MUCH more friendly process.  So today I&#8217;m excited to announce version 2.0 of the SkypeValidator API&#8217;s.

What can you do with them?  There are currently seven exposed API&#8217;s for you to access and use.  So let&#8217;s go through them:

<table width="100%">
  <tr>
    <td width="15%">
      <span style="text-decoration: underline"><strong>HTTP Method</strong></span>
    </td>
    
    <td width="35%">
      <span style="text-decoration: underline"><strong>Endpoint</strong></span>
    </td>
    
    <td>
      <span style="text-decoration: underline"><strong>Function</strong></span>
    </td>
  </tr>
  
  <tr>
    <td>
      GET
    </td>
    
    <td>
      <a href="https://masteringlync.com/checkcertificate/">/CheckCertificate</a>
    </td>
    
    <td>
      Validate state of certificate.  Thanks to DigiCert for powering this tool.
    </td>
  </tr>
  
  <tr>
    <td>
      GET
    </td>
    
    <td>
      <a href="https://masteringlync.com/checkdnsarecord/">/CheckDNSARecord</a>
    </td>
    
    <td>
      Check DNS A Record returns expected value
    </td>
  </tr>
  
  <tr>
    <td>
      GET
    </td>
    
    <td>
      <a href="https://masteringlync.com/checkdnssrvrecord/">/CheckDNSSrvRecord</a>
    </td>
    
    <td>
      Check DNS SRV Record returns expected value
    </td>
  </tr>
  
  <tr>
    <td>
      GET
    </td>
    
    <td>
      <a href="https://masteringlync.com/checkowas/">/CheckOwas</a>
    </td>
    
    <td>
      Check Office Web App Server returns expected value
    </td>
  </tr>
  
  <tr>
    <td>
      GET
    </td>
    
    <td>
      <a href="https://masteringlync.com/checkping/">/CheckPing</a>
    </td>
    
    <td>
      Check if you can ping server with return time
    </td>
  </tr>
  
  <tr>
    <td>
      GET
    </td>
    
    <td>
      <a href="https://masteringlync.com/checkport/">/CheckPort</a>
    </td>
    
    <td>
      Check is TCP or UDP port is open
    </td>
  </tr>
  
  <tr>
    <td>
      GET
    </td>
    
    <td>
      <a href="https://masteringlync.com/checksslcipher/">/CheckSSLCipher</a>
    </td>
    
    <td>
      Check what SSL Ciphers are open for an address
    </td>
  </tr>
</table>

So I&#8217;m in the process of documenting each of these but let&#8217;s review the SSL Cipher Check as an example.  So our available methods is a GET command and we would issue that against https://gcmsdk.azurewebsites.net/CheckSSLCipher/{guid}/{fqdn}/{port}

As you can see, there are three query string parameters for this command.

GUID: is a string value that is your API Key.  How do you get one, just e-mail me (richard -at- masteringlync.com).  The only reason I have the API key is so I know who is hitting the server.  There are no plans to charge for this but since it&#8217;s hosted in Azure and processing/network can cost money if you decide to make 1 billion requests in a month we might need to chat.

FQDN: should make sense.  It&#8217;s the FQDN.

PORT: what port are we checking on.

We can issue this via a curl command if we wanted to or via anything that can run a get command.  Our return value then is something like this:

<div class="codecolorer-container javascript default" style="overflow:auto;white-space:nowrap;width:95%;">
  <div class="javascript codecolorer">
    <span class="sy0"><</span>pre<span class="sy0">><</span>code <span class="kw5">class</span><span class="sy0">=</span><span class="st0">"language-bash"</span><span class="sy0">></span>curl – request GET \<br /> <span class="sy0">--</span>url <span class="st0">'https://gcmsdk.azurewebsites.net/CheckSSLCipher/{guid}/sipdir.online.lync.com/443'</span><br /> <span class="sy0">--</span>include<span class="sy0"></</span>code<span class="sy0">></</span>pre<span class="sy0">></span><br /> OR<br /> <br /> Powershell<span class="sy0">></span> Invoke<span class="sy0">-</span>RestMethod <span class="sy0">-</span>Url https<span class="sy0">:</span><span class="co1">//gcmsdk.azurewebsites.net/CheckSSLCipher/{guid}/sipdir.online.lync.com/443</span>
  </div>
</div>

And that would return:

<div class="codecolorer-container javascript default" style="overflow:auto;white-space:nowrap;width:95%;height:300px;">
  <div class="javascript codecolorer">
    <span class="br0">&#91;</span><br /> <span class="br0">&#123;</span><br /> <span class="st0">"FQDN"</span><span class="sy0">:</span><span class="st0">"sipdir.online.lync.com"</span><span class="sy0">,</span><br /> <span class="st0">"Port"</span><span class="sy0">:</span><span class="nu0">443</span><span class="sy0">,</span><br /> <span class="st0">"CipherSuite"</span><span class="sy0">:</span><span class="st0">"SSL 2.0"</span><span class="sy0">,</span><br /> <span class="st0">"CipherAlgorithm"</span><span class="sy0">:</span><span class="st0">"N/A"</span><span class="sy0">,</span><br /> <span class="st0">"Result"</span><span class="sy0">:</span><span class="st0">"Disabled"</span><br /> <span class="br0">&#125;</span><span class="sy0">,</span><br /> <span class="br0">&#123;</span><br /> <span class="st0">"FQDN"</span><span class="sy0">:</span><span class="st0">"sipdir.online.lync.com"</span><span class="sy0">,</span><br /> <span class="st0">"Port"</span><span class="sy0">:</span><span class="nu0">443</span><span class="sy0">,</span><br /> <span class="st0">"CipherSuite"</span><span class="sy0">:</span><span class="st0">"SSL 3.0"</span><span class="sy0">,</span><br /> <span class="st0">"CipherAlgorithm"</span><span class="sy0">:</span><span class="st0">"N/A"</span><span class="sy0">,</span><br /> <span class="st0">"Result"</span><span class="sy0">:</span><span class="st0">"Disabled"</span><br /> <span class="br0">&#125;</span><span class="sy0">,</span><br /> <span class="br0">&#123;</span><br /> <span class="st0">"FQDN"</span><span class="sy0">:</span><span class="st0">"sipdir.online.lync.com"</span><span class="sy0">,</span><br /> <span class="st0">"Port"</span><span class="sy0">:</span><span class="nu0">443</span><span class="sy0">,</span><br /> <span class="st0">"CipherSuite"</span><span class="sy0">:</span><span class="st0">"TLS 1.0"</span><span class="sy0">,</span><br /> <span class="st0">"CipherAlgorithm"</span><span class="sy0">:</span><span class="st0">"Aes256"</span><span class="sy0">,</span><br /> <span class="st0">"Result"</span><span class="sy0">:</span><span class="st0">"Enabled"</span><br /> <span class="br0">&#125;</span><span class="sy0">,</span><br /> <span class="br0">&#123;</span><br /> <span class="st0">"FQDN"</span><span class="sy0">:</span><span class="st0">"sipdir.online.lync.com"</span><span class="sy0">,</span><br /> <span class="st0">"Port"</span><span class="sy0">:</span><span class="nu0">443</span><span class="sy0">,</span><br /> <span class="st0">"CipherSuite"</span><span class="sy0">:</span><span class="st0">"TLS 1.1"</span><span class="sy0">,</span><br /> <span class="st0">"CipherAlgorithm"</span><span class="sy0">:</span><span class="st0">"Aes256"</span><span class="sy0">,</span><br /> <span class="st0">"Result"</span><span class="sy0">:</span><span class="st0">"Enabled"</span><br /> <span class="br0">&#125;</span><span class="sy0">,</span><br /> <span class="br0">&#123;</span><br /> <span class="st0">"FQDN"</span><span class="sy0">:</span><span class="st0">"sipdir.online.lync.com"</span><span class="sy0">,</span><br /> <span class="st0">"Port"</span><span class="sy0">:</span><span class="nu0">443</span><span class="sy0">,</span><br /> <span class="st0">"CipherSuite"</span><span class="sy0">:</span><span class="st0">"TLS 1.1"</span><span class="sy0">,</span><br /> <span class="st0">"CipherAlgorithm"</span><span class="sy0">:</span><span class="st0">"Aes256"</span><span class="sy0">,</span><br /> <span class="st0">"Result"</span><span class="sy0">:</span><span class="st0">"Enabled"</span><br /> <span class="br0">&#125;</span><br /> <span class="br0">&#93;</span>
  </div>
</div>

And there you have it. We can see that Microsoft is supporting TLS 1.0, 1.1 and 1.2 on their Edge Servers. Which makes sense as they have announced they will be disabling TLS 1.0 at the end of October.