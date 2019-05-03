---
id: 342
title: 'Lync CU2 &#8211; Question &amp; Answer Deep Dive'
date: 2013-07-10T05:16:58+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=342
permalink: /2013/07/10/lync-cu2-question-answer-deep-dive/
st_layout_box:
  - right
panels_data:
  - 'a:0:{}'
categories:
  - Lync Server 2013
---
In my [previous post](http://masteringlync.com/2013/07/09/lync-client-july-2013-update-cu2-details/) I detailed the new features that are part of the July 2013 Lync Server update (CU2) in a series of screenshots and although that can make valuable posting material its time to actually dig into how the product works.

So the first feature we will look into is the Question & Answer feature.  The thing that intrigues me the most about this new feature is that the Lync team is absolutely just taking advantage of platform.  Essentially, they are leveraging the conversation window/stage and doing a custom addins.  If we were doing custom development project, we would be leveraging the HKEY\_CURRENT\_USERSoftwareMicrosoftOffice15.0LyncAddins key to define our information.  I went and checked that location for giggles but there was nothing there as I assumed.

Lets go ahead and dig into what we can find.  We will use Fiddler as our primary tool for tracing.  When we start a conference and then click Q&A, we see this:

<a href="http://masteringlync.com/2013/07/10/lync-cu2-question-answer-deep-dive/pic1-8/" rel="attachment wp-att-343"><img class="alignnone wp-image-343 size-full" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/07/pic13.png?resize=701%2C193&#038;ssl=1" alt="pic1" width="701" height="193" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/07/pic13.png?w=701&ssl=1 701w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/07/pic13.png?resize=300%2C83&ssl=1 300w" sizes="(max-width: 701px) 100vw, 701px" data-recalc-dims="1" /></a>

The first thing you should realize is that this feature requires both the server side and client side patches to be applied to work.  Now that we know where the location is we should go and look and see what other apps are in that directory, any other surprises?

<a href="http://masteringlync.com/2013/07/10/lync-cu2-question-answer-deep-dive/pic2-6/" rel="attachment wp-att-344"><img class="alignnone wp-image-344 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/07/pic21.png?resize=183%2C189&#038;ssl=1" alt="pic2" width="183" height="189" data-recalc-dims="1" /></a>

The only new directory from CU1 to CU2 is the Qna directory so apparently no other secret features.  A subject for another day will be to dig through each and everyone of these directories.

The next step of the investigation is to figure out how questions and answers to passed back and forth to the client.  My first thought would have been we were using the same web services for this but that just isn&#8217;t the case.  Creating multiple questions and answers there was only one web request to:

<https://lyncfe03.thelab.info/DataCollabWeb/Qna/Resources/Replying.png>

So we go to the server and do some tracing.  The first thing that is interesting is to capture the C3P request to start Q&A which also stops group IM for everyone in the conference.  And if we stop the Q&A and start IM&#8217;s again, we see more traffic.

<a href="http://masteringlync.com/2013/07/10/lync-cu2-question-answer-deep-dive/pic3-6/" rel="attachment wp-att-345"><img class="alignnone wp-image-345 size-large" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/07/pic32-1024x284.png?resize=800%2C222&#038;ssl=1" alt="pic3" width="800" height="222" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/07/pic32.png?resize=1024%2C284&ssl=1 1024w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/07/pic32.png?resize=300%2C83&ssl=1 300w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/07/pic32.png?resize=768%2C213&ssl=1 768w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/07/pic32.png?w=1175&ssl=1 1175w" sizes="(max-width: 800px) 100vw, 800px" data-recalc-dims="1" /></a>

Here we can see msci being modified from false to true and block to unblock.  Fairly interesting stuff overall.  As for the actual questions and answers, I have gotten Wireshark to play nice in my lab yet so I can decrypt the traffic, however I&#8217;m seeing PSOM/8057 traffic going back and forth when I ask/answer questions.  So maybe its hiding in there.  Will update as soon as I can get it to decrypt again.

&nbsp;