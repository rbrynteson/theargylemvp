---
id: 601
title: Revisiting spell check in Lync 2013
date: 2014-01-13T02:43:55+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=601
permalink: /2014/01/13/revisiting-spell-check-in-lync-2013/
st_layout_box:
  - right
panels_data:
  - 'a:0:{}'
categories:
  - Lync Server 2013
---
Last year when CU3 was released I posted a quick article on how to get [spell check](http://masteringlync.com/2013/09/26/lync-2013-spell-check-why-doesnt-it-work/) to work with Lync 2013 and Office 2010/2007 installed on the computer.  Since than, got a lot of great feedback and I wanted to pass along this find.  Credit goes completely to a reader (Brendan the true Captain Jack) for finding yet another way to get this to work.

If you are installing the Lync 2013 client via the &#8220;stand-alone package&#8221; and not using the full Office Pro Plus installer there are some options you want to be careful not to disable.  Below is a screenshot of the Microsoft Office Customization Tool which can be used to created your MSP for the Lync 2013 Stand-Alone installer &#8211; you will see Proofing Tools as an option.

[<img class="alignnone wp-image-602 size-full" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/01/1.jpg?resize=624%2C356&#038;ssl=1" alt="1" width="624" height="356" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/01/1.jpg?w=624&ssl=1 624w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/01/1.jpg?resize=300%2C171&ssl=1 300w" sizes="(max-width: 624px) 100vw, 624px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.com/files/2014/01/1.jpg)

Within the MSP, you have the ability to customize and include the Office Shared Features | Proofing Tools directly within the installer.  If you are working on your Lync client deployment for the first time, you should make sure to include these features.

**What if I have Lync already installed without them?**

You have two options:

1. Go to Programs and Features and select Lync 2013 and choose &#8220;Change&#8221; and select Add or Remove Features and add it back in.

2. Command line version.

  * Close the Lync Client
  * Open up the command prompt
  * Run this
  * msiexec.exe /I{90150000-012C-0000-0000-0000000FF1CE} /QB+ ADDSOURCE=ProofingParent
  * And than this
  * msiexec.exe /I{90150000-001F-0409-0000-0000000FF1CE} /QB+ ADDLOCAL=ALL
  * Launch Lync and you should be good to go

For French language you have msiexec.exe /I{90150000-001F-040C-0000-0000000FF1CE} /QB+ ADDLOCAL=ALL and Spanish is msiexec.exe /I{90150000-001F-0C0A -0000-0000000FF1CE} /QB+ ADDLOCAL=ALL

Enjoy and happy spell checking!