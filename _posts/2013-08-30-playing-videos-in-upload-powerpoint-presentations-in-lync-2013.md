---
id: 367
title: Playing Videos in Uploaded PowerPoint Presentations in Lync 2013
date: 2013-08-30T03:05:23+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=367
permalink: /2013/08/30/playing-videos-in-upload-powerpoint-presentations-in-lync-2013/
st_layout_box:
  - right
categories:
  - Lync Server 2013
---
One of the new features of Lync Server 2013 is the ability to leverage the Office Web App Server (OWAS) to have a better experience when presenting PowerPoint presentations and in particular embedding video within those presentations.  Along with this comes some new requirements that you might not have considered before.

Version of Internet Explorer.  According to the Lync 2013 Client Requirements IE 7 or later is all that needed:

http://technet.microsoft.com/en-us/library/gg412781.aspx

However, if you plan on supporting video within your PowerPoint presentations your real requirements are IE9 or later.  This is due to what OWAS is really doing.  When you upload a PowerPoint into OWAS it is converting the document into a web based version that can easily be consumed by various clients/devices.  Videos in particular are converted into HTML5 elements.  This means that your browser used with your Lync 2013 client needs to support HTML5, which means IE9 or later only.  The user experience when you view this video with IE8 or older will be to display the first frame of the movie but none of the movie controls are available.  For some organization this can be a BIG deal.  Many large organizations are still using IE8 on Windows 7.

Version of Office/PowerPoint.  The version of PowerPoint and the video format can also have a large impact on the user experience.  Based on my testing over the last day I have found the following items:

First, any files created with PowerPoint 2007 or earlier will not play the video when uploaded into OWAS.  Not entirely unexpected as PowerPoint 2007 is pretty old.  But what about the more recent versions.  Most companies are still using 2010 and even though many companies are deploying Lync Server 2013 and the 2013 Client, they are staying on Office 2010 because of various reasons.

So let&#8217;s take a simple example of WMV.  A user creates a PowerPoint in 2010/2013 and uploads it to a Lync Meeting and you find that the video will not play.  You will get an error which will bring you to this page:

http://office.microsoft.com/en-us/web-apps-help/play-a-video-in-powerpoint-web-app-HA103361387.aspx

The key to this post is at the bottom of the post.

_When you add a video to your presentation in PowerPoint 2013, make it easy for people to play in PowerPoint Web App. Embed an MP4 file, and then go to **File** > **Optimize Media Compatibility**. This command puts your video in a format that plays directly in browsers that support HTML5._

However, not all Optimize Media Compatibilities are built the same.  Both PowerPoint 2010 and 2013 offer this feature however based on testing I found that they don&#8217;t work the same.

<div>
  <table width="100%" border="1" cellspacing="0" cellpadding="0">
    <tr>
      <td>
        File Type
      </td>
      
      <td>
        PowerPoint 2010 Not Optimized
      </td>
      
      <td>
        PowerPoint 2010 W/ Optimization
      </td>
      
      <td>
        PowerPoint 2013 Not Optimized
      </td>
      
      <td>
        PowerPoint 2013 W/ Optimization
      </td>
    </tr>
    
    <tr>
      <td>
        WMV
      </td>
      
      <td>
        Does Not Work
      </td>
      
      <td>
        Option Not Available
      </td>
      
      <td>
        Does Not Work
      </td>
      
      <td>
        Works
      </td>
    </tr>
    
    <tr>
      <td>
        ASF
      </td>
      
      <td>
        Does Not Work
      </td>
      
      <td>
        Does Not Work
      </td>
      
      <td>
        Does Not Work
      </td>
      
      <td>
        Works
      </td>
    </tr>
    
    <tr>
      <td>
        AVI
      </td>
      
      <td>
        Does Not Work
      </td>
      
      <td>
        Does Not Work
      </td>
      
      <td>
        Does Not Work
      </td>
      
      <td>
        Works
      </td>
    </tr>
    
    <tr>
      <td>
        MPEG1/2
      </td>
      
      <td>
        Does Not Work
      </td>
      
      <td>
        Does Not Work
      </td>
      
      <td>
        Does Not Work
      </td>
      
      <td>
        Works
      </td>
    </tr>
    
    <tr>
      <td>
        MP4
      </td>
      
      <td>
        Does Not Work
      </td>
      
      <td>
        Does Not Work
      </td>
      
      <td>
        Does Not Work
      </td>
      
      <td>
        Option Not Available
      </td>
    </tr>
    
    <tr>
      <td>
        SFW
      </td>
      
      <td>
        Does Not Work
      </td>
      
      <td>
        Option Not Available
      </td>
      
      <td>
        Does Not Work
      </td>
      
      <td>
        Option Not Available
      </td>
    </tr>
    
    <tr>
      <td>
        M4V
      </td>
      
      <td>
        Does Not Work
      </td>
      
      <td>
        Does Not Work
      </td>
      
      <td>
        Works
      </td>
      
      <td>
        Works
      </td>
    </tr>
  </table>
</div>

&nbsp;

As you can see in the above table using PowerPoint 2010 resulted in no videos actually playing in the Lync Meeting experience.  
Even when using the Optimization option.  When we turn our attention to PowerPoint 2013 we can see several formats don&#8217;t work without first using the media optimization.  This can get even more complicated.  What if you create the presentation in 2010 but later open it in 2013 and use the optimization feature.  Based on my testing, all of these videos will play just as the table above shows.  Lastly, optimizing video in 2010 doesn&#8217;t prevent you from doing it again if you happen to open the file in 2013 later.

Like many things within the realm of Lync there are things that are well documented and others that you must go find on your own.  Make sure to understand the exact requirements for using this feature and ensure users who plan to use this feature have their source videos in the correct format.

NOTES:

I did all of this testing with the same set of media files.  There are of course multiple rates/etc that a single file can be saved in so if you find one that works let me know.

&nbsp;