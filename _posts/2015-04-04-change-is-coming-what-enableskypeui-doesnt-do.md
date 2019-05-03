---
id: 1119
title: 'Change is coming.  What EnableSkypeUI doesn&#8217;t do.'
date: 2015-04-04T16:40:15+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=1119
permalink: /2015/04/04/change-is-coming-what-enableskypeui-doesnt-do/
panels_data:
  - 'a:0:{}'
categories:
  - Lync Server 2013
  - Skype for Business
---
Microsoft has done a good job of telling people that change is coming to their Lync client. A quick recap of what is coming.

April 14th (Patch Tuesday) Microsoft is starting the process of pushing out the Skype for Business Client UI. This is going to hit &#8220;click to run&#8221; and those users who use Windows Update first. If you are a business organization you have more control because you are most likely using SCCM/WSUS to approve patches to your workstations and you can take a bit more time to deal with it.

Microsoft has several resource sites that you can use for more details on this. I suggest checking these out:

  * <a href="https://support.office.com/en-us/article/Skype-for-Business-change-management-and-adoption-d8d85da6-52e7-4819-8451-45c103fb5ccb?omkt=en-us&ui=en-US&rs=en-US&ad=US" target="_blank">Skype for Business change management and adoption</a>
  * <a href="https://technet.microsoft.com/en-us/library/dn913785.aspx" target="_blank">TechNet Article</a>
  * <a href="http://www.microsoft.com/en-us/download/details.aspx?id=46369" target="_blank">Skype for Business Readiness Resources</a>

In these you will find the EnableSkypeUI option that allows you to change the <a href="http://masteringlync.com/2015/03/16/skype-for-business-for-lync-ui-changes/" target="_blank">UI </a>of your client.  However, there are a few things that will change regardless that the EnableSkypeUI doesn&#8217;t control.

**Sounds**

The installer will replace all of the Lync Sounds with the Skype Sounds.  Users of both UI&#8217;s will experience this change.  To me, this is the most notable change that your users will notice regardless.  The sounds are VERY different then before.  You should make sure your users are aware of this and your helpdesk is ready for the calls.

**Meeting Links**

If you are using Lync Conferencing your users are about to see a change.  The Lync patch will update the Outlook Plugin and change the Lync Meeting to a Skype Meeting

[<img class="alignnone size-full wp-image-1120" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/04/1.png?resize=237%2C306&#038;ssl=1" alt="1" width="237" height="306" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/04/1.png?w=237&ssl=1 237w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/04/1.png?resize=232%2C300&ssl=1 232w" sizes="(max-width: 237px) 100vw, 237px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/04/1.png)

And the actual join link will be changed as well

[<img class="alignnone size-full wp-image-1121" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/04/2.png?resize=433%2C91&#038;ssl=1" alt="2" width="433" height="91" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/04/2.png?w=433&ssl=1 433w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/04/2.png?resize=300%2C63&ssl=1 300w" sizes="(max-width: 433px) 100vw, 433px" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/04/2.png)

**Icons**

Your Lync icon will look a lot like Skype for Business icon regardless which UI you choose.

[<img class="alignnone size-full wp-image-1130" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/04/4.png?resize=61%2C44&#038;ssl=1" alt="4" width="61" height="44" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/04/4.png)

**Title Bar**

The title is going to say Skype for Business.  If you have the Lync UI, it will say _Skype for Business (Lync Mode)._

**Initial Join**

The default behavior of the Skype for Business Client is to launch into the Skype for Business UI.  If you have selected via policy to use the Lync 2013 client you will see this:

[<img class="alignnone size-full wp-image-1124" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/04/3.png?resize=355%2C239&#038;ssl=1" alt="3" width="355" height="239" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/04/3.png?w=355&ssl=1 355w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/04/3.png?resize=300%2C202&ssl=1 300w" sizes="(max-width: 355px) 100vw, 355px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/04/3.png)

This poses two problems.  First, you need to inform all of your users they are going to see this annoying message.  Second, it gives them the ability to choose Restart Later and lets them play in the new UI (which they are going to pick because end-users who see Restart will always pick later).  The only way to suppress this is to push out the registry key before hand so the client never even asks.  See this <a href="http://masteringlync.com/2015/03/16/skype-for-business-for-lync-ui-changes/" target="_blank">post for registry settings</a>.

Here is a table that describes the scenarios

[<img class="alignnone size-column212 wp-image-1128" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/04/21.png?resize=599%2C310&#038;ssl=1" alt="2" width="599" height="310" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/04/21.png?w=599&ssl=1 599w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/04/21.png?resize=300%2C155&ssl=1 300w" sizes="(max-width: 599px) 100vw, 599px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/04/21.png)

This is the most notable visual change that users will see after the patch if they are using the Lync UI option.  Hopefully these help and if I find anymore changes along the way I&#8217;ll make sure to note them here.