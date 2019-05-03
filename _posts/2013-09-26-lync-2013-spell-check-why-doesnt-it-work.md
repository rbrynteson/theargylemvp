---
id: 391
title: 'Lync 2013 Spell Check &#8211; Why doesn&#8217;t it work?'
date: 2013-09-26T17:58:01+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=391
permalink: /2013/09/26/lync-2013-spell-check-why-doesnt-it-work/
st_layout_box:
  - right
categories:
  - Lync Server 2013
---
It&#8217;s well documented that the September 2013 Lync Client update has brought us spell check natively within the Lync client.

http://flinchbot.wordpress.com/2013/09/13/lync-2013-spell-check/

It&#8217;s also well documented that this patch breaks calendar integration which is absolutely mind blowing that a bug of that size made it through quality testing but that is a discussion for another time.  However what isn&#8217;t documented is what are the requirements of this feature.

Office has had a long history of not supporting spell check between product versions.  For example, back when Exchange 2010 came out everyone wanted the &#8220;cool features&#8221; of Outlook 2010 but didn&#8217;t want to move the rest of the Office suite so it was very common to install Office 2007 and Outlook 2010.  But in this scenario, spell check didn&#8217;t work within Outlook 2010 and you had to resort to crazy work-arounds like moving winword.exe from the Office12 to Office14 directory.

<div class="fitvids-video">
  <blockquote data-secret="EGTVYfl4MI" class="wp-embedded-content">
    <p>
      <a href="http://blog.lanlogic.net/blog/spell-check-in-outlook-2010-not-working/">Spell Check in Outlook 2010 not working?</a>
    </p>
  </blockquote>
  
  <p>
    </div> 
    
    <p>
      So I was curious to see if this new spell check feature resulted with the same issues and indeed it does.  One of the most common deployments for Lync 2013 is Office 2010 + Lync 2013 Client.  In this scenario, even with the fancy new September 2013 patch, spell check simply does not work.  The option exists in the client but you will never see the red-lines.
    </p>
    
    <p>
      I decided there had to be a way to make this work in 2013.  So I figured, does the old trick still work?  So I moved WinWord.exe from the Office14 to Office15 folder to see if spell check would work and it did not.  So I started doing some research and found that in Office 2013 the team moved spell check to it&#8217;s own modules.  A smart move.  And further research found that not only is it a separate module but a stand-alone installer exists.
    </p>
    
    <p>
      So if you are using Lync 2013 client with either Office 2010 or Office 2007, you can install the stand-alone proofing tools to get the feature to work.
    </p>
    
    <p>
      x64 &#8211; http://download.microsoft.com/download/D/0/D/D0D1D4E2-0A26-44EE-A9A1-C987217BBC2C/proofingtools_en-us-x64.exe
    </p>
    
    <p>
      x86 &#8211; http://download.microsoft.com/download/D/0/D/D0D1D4E2-0A26-44EE-A9A1-C987217BBC2C/proofingtools_en-us-x86.exe
    </p>