---
id: 71
title: Centralized Logging in Lync 2013
date: 2012-08-02T03:46:41+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=71
permalink: /2012/08/02/centralized-logging-in-lync-2013/
st_layout_box:
  - right
panels_data:
  - 'a:0:{}'
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
categories:
  - Lync Server 2013
tags:
  - Lync
  - Lync Server 2013
  - Wave 15
---
So one of the items that may make some people uncertain at first, is the fact that OCSLogger is missing from servers and it doesn&#8217;t sound like it&#8217;s coming back anytime soon.  What replaces it now, is Centralized Logging Service (CLS).  The major difference between CLS and the old method is that when you do a trace, it occurs on all servers in the pool at once &#8211; hence the term centralized.

All of this is controlled by the CLSAgent that is running on every server.  I haven&#8217;t dug into the edge components yet, but will update and report back as soon as I get more information on how you connect to it, etc.

Now CLS is controlled via a command line process which looks like this:

  * <span style="font-family: 'Courier New';font-size: small">ClsController.exe -start –scenario <scenario> –pools <pool fqdn></span>
  * <span style="font-size: small">Reproduce the Issue</span>
  * <span style="font-family: 'Courier New';font-size: small">ClsController.exe -stop –scenario <scenario> –pools <pool fqdn></span>
  * <span style="font-family: 'Courier New';font-size: small">ClsController.exe -flush –pools <pool fqdn></span>
  * <span style="font-family: 'Courier New';font-size: small">ClsController.exe -search –pools <pool fqdn> –components <component> –loglevel <loglevel></span>

Check out Jens Trier Rasmussen&#8217;s [blog article](http://blogs.technet.com/b/jenstr/archive/2012/08/01/using-centralized-logging-service-in-lync-server-2013-preview.aspx) on the details of each scenario, etc.

So Jens has detailed the process really well, so what exactly am I blogging about at this point in time?  How to automate the process a bit.  I&#8217;ve very impressed with CLS and like it a bunch but I know that I don&#8217;t stand a chance of memorizing all of the scenario values.  My guess is that we will see an official GUI at some point in time that is basically doing what I&#8217;m doing here, but figured I would toss something together quickly as I have some tracing to do.

So what I have done is created a DOS Batch file (yep &#8211; remember DOS).  So how does it work:

>LyncLogging.bat <poolname> <filename>

[<img class="alignnone wp-image-72 size-full" title="ScreenShot" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2012/08/ScreenShot.png?resize=588%2C354&#038;ssl=1" alt="" width="588" height="354" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2012/08/ScreenShot.png?w=588&ssl=1 588w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2012/08/ScreenShot.png?resize=300%2C181&ssl=1 300w" sizes="(max-width: 588px) 100vw, 588px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.com/files/2012/08/ScreenShot.png)

At the end, the output will create the trace file which can be opened up via Snooper.  You can download my very <del>un</del>fancy batch file [here](http://masteringlync.com/wp-content/uploads/sites/2/2012/08/LyncLogging.txt).

You need to rename the LyncLogging.txt to LyncLogging.bat.

I will be updating the script at some point in time so you can limit the search results to a smaller subset using the components command.  Might also move it to PowerShell GUI option as well.  Been looking for a good project to drop into PowerShell.

Hopefully you find it somewhat useful.

UPDATED: Ports 50001 to 50003 need to be open for each server.  Windows Firewall is modified as part of the installer, but if you have a DMZ to the edge, then you need to be opening up these ports for Edge Traffic.

&nbsp;