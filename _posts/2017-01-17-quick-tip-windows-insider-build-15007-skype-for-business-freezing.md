---
id: 1357
title: 'Quick Tip: Windows Insider Build 15007 + Skype for Business = Freezing'
date: 2017-01-17T13:32:16+00:00
author: Richard Brynteson
layout: post
guid: http://masteringlync.com/?p=1357
permalink: /2017/01/17/quick-tip-windows-insider-build-15007-skype-for-business-freezing/
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
  - Skype for Business
  - WindowsInsider
---
This will be indeed be a quick tip.  Last Friday I updated to Windows Insider Build 15007.  I love being on the bleeding edge of technology but every once in a while things don&#8217;t go the way you want them.  Monday morning I found that SfB Client wasn&#8217;t loading anymore.  Now I&#8217;m in the Insider Fast for Office as well (I could open Outlook Meetings and more).  So I assumed it was a new build of Office and rolled that back to production.  The problem continued.

I jumped over to EventViewer to see if there was anything useful &#8211; typically there is not and I found this:

<a href="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2017/01/1.png" rel="attachment wp-att-1358"><img class="alignnone wp-image-1358 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2017/01/1.png?resize=590%2C328&#038;ssl=1" alt="1" width="590" height="328" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2017/01/1.png?w=590&ssl=1 590w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2017/01/1.png?resize=300%2C167&ssl=1 300w" sizes="(max-width: 590px) 100vw, 590px" data-recalc-dims="1" /></a>

Figured having whatever Spectrum.exe crashing every second wasn&#8217;t a good thing.  So a bit of hunting around and I found this post:

https://answers.microsoft.com/en-us/insider/forum/insider\_wintp-insider\_devices/build-15007-no-audio-issue-workaround/c4c931ff-0201-42b5-8725-998a3e22c803

Which tells us to blow away a folder and restart:

<pre style="padding-left: 30px">Rmdir /s %ProgramData%\Microsoft\Spectrum\PersistedSpatialAnchors</pre>

<pre style="padding-left: 30px">Shutdown /r</pre>

After the reboot all was good.  Now to put the Office Insiders Fast ring back on.

&nbsp;