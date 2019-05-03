---
id: 348
title: AD/Lync/Exchange 2013 Provisioning Script
date: 2013-07-30T15:15:57+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=348
permalink: /2013/07/30/adlyncexchange-2013-provisioning-script/
st_layout_box:
  - right
categories:
  - Exchange 2013
  - Lync Server 2013
---
So I took a moment to write a provisioning script for Active Directory/Exchange/Lync.  It&#8217;s a pretty straight forward script and I&#8217;m no PowerShell expert so I don&#8217;t pretend this is the best way to write it.  So what the script does is:

  1. Prompts you to which OU you want to create the user.
  2. It will create/do the following: 
      * Create an AD User Object
      * Enable the User for Exchange
      * Enable the User for Lync
      * Enable the User for Exchange Unified Messaging
  3. You need to provide the following via variables: 
      * Location of Exchange
      * Location of Lync
      * Default Voice Policy
      * Default UM Policy
      * Default Pin
      * Default Password

The script is available here for download <a href="http://masteringlync.com/2013/07/30/adlyncexchange-2013-provisioning-script/script/" rel="attachment wp-att-349">here</a>.