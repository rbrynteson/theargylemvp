---
id: 646
title: Introducing the LyncValidator
date: 2014-02-16T23:08:45+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=646
permalink: /2014/02/16/introducing-the-lyncvalidator/
st_layout_box:
  - right
panels_data:
  - 'a:0:{}'
categories:
  - Lync Server 2010
  - Lync Server 2013
---
Over the past few months I&#8217;ve started working on a project to help validate the design and implementation of Lync Projects.  It was designed to not only give you information about the deployment but to ensure that you don&#8217;t do silly things along way.  The LyncValidator is an online tool that allows you to define the parameters of your Lync deployment and return critical information.  My hope is to build this enough to combine the power of tools such as the &#8220;Planning Tool&#8221;, Test Connectivity Tools, TechNet information and much more. So this is a quick run down of what is possible today within the web application.  If you have thoughts of additional tools you would like to see added, please let me know and I&#8217;ll add it to the wish list. **Design Information** Currently today, we allow you to create your topology with the following information:

  * Support for up to 5 SIP Domains
  * Support for unlimited Standard or Enterprise Front-End Pools
  * Support for unlimited Edge Pools with up to 6 servers within each pool
  * Support for unlimited OWAS Farms with up to 4 Servers within each farm
  * QoS Client and Server Settings

In the future we will add the remaining roles such as separate mediation pools and director pools but frankly those are a little lower on my priority list of as today. [<img class="alignnone wp-image-647" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/02/pic11-300x70.png?resize=600%2C140&#038;ssl=1" alt="pic1" width="600" height="140" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/02/pic11.png?resize=300%2C70&ssl=1 300w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/02/pic11.png?resize=768%2C179&ssl=1 768w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/02/pic11.png?w=989&ssl=1 989w" sizes="(max-width: 600px) 100vw, 600px" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.com/files/2014/02/pic11.png)

**Output Information** The application will output the necessary information for your Lync design, including:

  * Internal and External DNS
  * Internal and External Certificates
  * External Firewall
  * Edge and Reverse Proxy Hosts File
  * PowerShell and Scripts to: 
      * PowerShell to create Server and Client QoS Settings
      * PowerShell to create Group Policy Objects for QoS Settings
      * PowerShell commands to deploy OWAS Server(s)
      * Command prompt to create internal DNS Settings
      * Command prompt to create DHCP Settings for Aries Phones

**Validation Information** This is the part of the tool that I&#8217;ve enjoyed writing the most to be honest.  Because knowing what information you need to put into your design is important but verifying that the information is correct is critical. _Design Validation_ In this section we will go through the options you have selected to ensure that you haven&#8217;t done anything that would violate a best practice.  For example, creating a front-end pool with only two servers.  Creating a QoS policy with too few ports.  Just a snapshot of the items we will validate for and this list continues to grow each week. [<img class="alignnone wp-image-648 size-medium" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/02/pic2-300x87.png?resize=300%2C87&#038;ssl=1" alt="pic2" width="300" height="87" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/02/pic2.png?resize=300%2C87&ssl=1 300w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/02/pic2.png?resize=768%2C223&ssl=1 768w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/02/pic2.png?w=992&ssl=1 992w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.com/files/2014/02/pic2.png) _Deployment Validation_ In this section we will validate the information entered into the designer actually is validated to the real-world deployment.  For example, we will ensure that host records resolve to the IP&#8217;s addresses listed in the design document (limited to A records today &#8211; SRV is coming).  We will check TCP Ports are open on your firewall to Edge and Reverse Proxy.  Validate OWAS servers return the correct WOPI information when hit remotely.  And lastly, with the help of DigiCert, we will validate your certificates are installed correctly and the certificate chain is valid as well. [<img class="alignnone wp-image-649 size-medium" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/02/pic3-300x121.png?resize=300%2C121&#038;ssl=1" alt="pic3" width="300" height="121" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/02/pic3.png?resize=300%2C121&ssl=1 300w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/02/pic3.png?resize=768%2C310&ssl=1 768w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/02/pic3.png?w=982&ssl=1 982w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.com/files/2014/02/pic3.png) This is another section where we are growing the list of items we are validating all the time. **Other Notes** Its worth noting that this is an online tool, which will require you to provide your Microsoft Live ID and Password in order to use.  This allows us to save your deployment information from session to session.  I make a promise that I&#8217;ll never look or use that information for anything &#8211; and if you wish &#8211; you can delete your deployment information at anytime. Right now, the SQL information is stored in a Windows Azure database and in the very near future, the web services will be moved to Windows Azure as well. The application allows you to export the information to a Word Document when you are finished as well. If you are interested in playing around with it, check out the tool at http://lyncvalidator.com If you have feedback feel free to reach out to myself ([@rbrynteson](http://twitter.com/rbrynteson)) or Michael LaMontagne ([@_mlamontagne](http://twitter.com/_mlamontagne)).