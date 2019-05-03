---
id: 1170
title: Using Call Quality Dashboard (Part One)
date: 2015-05-20T03:48:14+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=1170
permalink: /2015/05/20/using-call-quality-dashboard-part-one/
panels_data:
  - 'a:0:{}'
categories:
  - Skype for Business
---
This is going to be a multi-part series on using the Call Quality Dashboard.  I don&#8217;t have a sense of how many articles there will be but I promise more than one and less then 100.

**Overview**

The Call Quality Dashboard allows you as an administrator to look your QoE/CDR information in a different tool.  The interesting thing about the CQD is the ability to modify and create your own reports.  The goal of this series of posts is to see how many and the types of reports I can create within the free CQD system.

**Monthly Summary Report**

The first report I am going to create is a monthly stream report.  This report is going to show me a single bar graph for each month that display the audio, video and app sharing streams in my organization.

  1. Go to the Call Quality Dashboard
  2. Fine the Audio Streams Monthly Trend report on the home page and click Clone.  This will create a new report to the right of all existing reports.  Click on edit.  
<img class="alignnone" src="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/01.png?resize=766%2C515&#038;ssl=1" alt="" width="766" height="515" data-recalc-dims="1" /> 
  3. There are three sections you can edit within the report.The first is the dimension, measurements and filters (red box).  For this first graph we are creating you can leave dimensions and filters.  Under measurements, remove all of the existing measurements and add the following: Audio Streams Count, Video Streams Count and App Sharing Streams Count. 
    In the top section (blue box), you want to make sure that Chart and Trend are both selected.
    
    In the bottom section (green box) you should click edit and modify the report name and description.  Name: Monthly Streams Report.  Description: This report shows audio, video and app sharing stream counts by Month.
    
<img class="alignnone" src="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/04.png?resize=765%2C512&#038;ssl=1" alt="" width="765" height="512" data-recalc-dims="1" /> 
    
    The final report configuration should look like the above.</li> </ol> 
    
    Your final report should look like the following.  The report will show all audio, video and app sharing streams in a single bar graph over a monthly trend.
    
<img class="alignnone" src="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/02.png?resize=612%2C591&#038;ssl=1" alt="" width="612" height="591" data-recalc-dims="1" /> 
    
    **Daily Summary Report**
    
    In our second report, we are going to dive into our monthly summary report we created above and add a last 30 days summary report.
    
      1. Since we want this report to be a sub report of the monthly summary report, click the Add Sub Report button 
<img class="alignnone" src="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/03.png?resize=188%2C36&#038;ssl=1" alt="" width="188" height="36" data-recalc-dims="1" /> 
        
        This will copy the Monthly Summary Report and add it as a sub report.</li> 
        
          * Click Edit.  Change the Dimension from StartDate.Month to StartDate.Date.  Edit the title and description of the report.</ol> 
        
        Your final report should look like the following.  The report will show all audio, video and app sharing streams in a single bar graph daily over a month.
        
<img class="alignnone" src="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/05.png?resize=582%2C576&#038;ssl=1" alt="" width="582" height="576" data-recalc-dims="1" /> 
        
        That is the start of our report package created with CQD.  As more are created I&#8217;ll update this post.
        
        &nbsp;