---
id: 506
title: 'Lync Client CU3 (November Update) &#8211; Show a picture from a website!!!'
date: 2013-11-11T01:17:40+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=506
permalink: /2013/11/11/lync-client-cu3-november-update-show-a-picture-from-a-website/
st_layout_box:
  - right
panels_data:
  - 'a:0:{}'
categories:
  - Lync Server 2013
---
The Lync 2013 Client Update (CU3) November is out.

KB: <http://support.microsoft.com/kb/2825630/en-us>

Download: <http://support.microsoft.com/kb/2825630/en-us>

It has several new features which can be found here:

http://lyncdup.com/2013/11/november-lync-2013-client-update-15-0-4551-1005-exchange-freebusy-and-recording-options/

  * Spell Check without breaking presence
  * Recording Options
  * Return of Web Photo Option

The last one needs to be enabled via some PowerShell magic to make it work.  Run the following:

$PolicyEntry=New-CsClientPolicyEntry -Name EnablePresencePhotoOptions -Value true  
$currentClientPolicy=Get-CsClientPolicy -Identity Global  
$currentClientPolicy.PolicyEntry.Add($PolicyEntry)  
Set-CsClientPolicy -Instance $currentClientPolicy

Credit goes to [Jen&#8217;s](http://blogs.technet.com/b/jenstr/) as he posted similar information several months ago but the post disappeared which I assume had something to do with the feature being pushed back to this patch.

<a href="http://masteringlync.com/2013/11/11/lync-client-cu3-november-update-show-a-picture-from-a-website/pic3-12/" rel="attachment wp-att-507"><img class="alignnone wp-image-507 size-medium" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/11/pic31-300x245.png?resize=300%2C245&#038;ssl=1" alt="pic3" width="300" height="245" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/11/pic31.png?resize=300%2C245&ssl=1 300w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/11/pic31.png?w=731&ssl=1 731w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" /></a>

Enjoy the return of Show a Picture from a website

UPDATE 8:20 PM

Pictures are EVERYWHERE NOW!!!  See the IM conversation.

<a href="http://masteringlync.com/2013/11/11/lync-client-cu3-november-update-show-a-picture-from-a-website/pic3-13/" rel="attachment wp-att-510"><img class="alignnone wp-image-510 size-full" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/11/pic32.png?resize=430%2C432&#038;ssl=1" alt="pic3" width="430" height="432" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/11/pic32.png?w=430&ssl=1 430w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/11/pic32.png?resize=150%2C150&ssl=1 150w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/11/pic32.png?resize=300%2C300&ssl=1 300w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/11/pic32.png?resize=100%2C100&ssl=1 100w" sizes="(max-width: 430px) 100vw, 430px" data-recalc-dims="1" /></a>

Next to every IM with someone.  That might be a little too much