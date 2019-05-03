---
id: 828
title: 'Quick Tip: Automating SQL Reporting Reports'
date: 2014-09-16T14:50:16+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=828
permalink: /2014/09/16/quick-tip-automating-sql-reporting-reports/
st_layout_box:
  - right
panels_data:
  - 'a:0:{}'
categories:
  - Lync Server 2013
---
There are a ton of great tools available from both Microsoft and third party providers for reporting information about your Lync deployment.  One of the often overlooked options we have is to automate the delivery of reports from your SQL Reporting Server.  Here are the quick steps to enable this feature.

**Step #1 &#8211; Setup SMTP on your SQL Reporting Server**

For whatever reason, my experience is SQL Administrators never setup the SMTP options for the SRS server.  To configure this, launch the SQL Reporting Server Configuration tool on your SQL Server.  Go to the E-mail Settings and enter in your Sender Address, Current SMTP Delivery Method and SMTP Server.  When you have finished entering those settings, click on Apply at the bottom.

[<img class="alignnone wp-image-829 size-full" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/09/pic11.png?resize=741%2C565&#038;ssl=1" alt="pic1" width="741" height="565" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/09/pic11.png?w=741&ssl=1 741w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/09/pic11.png?resize=300%2C229&ssl=1 300w" sizes="(max-width: 741px) 100vw, 741px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.com/files/2014/09/pic11.png)

Make sure that the SMTP Server is setup to accept anonymous relay as there are no authentication options available from this management interface.

**Step #2 &#8211; Subscribe to your reports**

In order to subscribe to the reports, you need to make sure that you go to the reporting server page and not the reporting page.  Typically it will be a name like this:  http://<serverfqdn>/Reports_<instancename>.

You should see a page similar to this.

[<img class="alignnone wp-image-830 size-medium" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/09/pic21-300x82.png?resize=300%2C82&#038;ssl=1" alt="pic2" width="300" height="82" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/09/pic21.png?resize=300%2C82&ssl=1 300w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/09/pic21.png?resize=768%2C209&ssl=1 768w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/09/pic21.png?w=1010&ssl=1 1010w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.com/files/2014/09/pic21.png)

Click on LyncServerReports, Reports_Content and then on the far right side click on Detail View.  You should see something like this:

[<img class="alignnone wp-image-831 size-large" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/09/pic31-1024x230.png?resize=800%2C180&#038;ssl=1" alt="pic3" width="800" height="180" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/09/pic31.png?resize=1024%2C230&ssl=1 1024w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/09/pic31.png?resize=300%2C67&ssl=1 300w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/09/pic31.png?resize=768%2C172&ssl=1 768w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/09/pic31.png?w=1600&ssl=1 1600w" sizes="(max-width: 800px) 100vw, 800px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.com/files/2014/09/pic31.png)

At this point in time, you should see a list of all of the reports available to you.  Find the report you wish to subscribe to and hover over it.  You should see an arrow and select subscribe:

[<img class="alignnone wp-image-832 size-medium" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/09/pic4-300x208.png?resize=300%2C208&#038;ssl=1" alt="pic4" width="300" height="208" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/09/pic4.png?resize=300%2C208&ssl=1 300w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/09/pic4.png?w=472&ssl=1 472w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.com/files/2014/09/pic4.png)

On the resulting page you will have the option to e-mail the reports to yourself.

[<img class="alignnone wp-image-833 size-medium" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/09/pic5-300x282.png?resize=300%2C282&#038;ssl=1" alt="pic5" width="300" height="282" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/09/pic5.png?resize=300%2C282&ssl=1 300w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/09/pic5.png?w=709&ssl=1 709w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.com/files/2014/09/pic5.png)

In addition to the e-mail option, you can also select a file location to drop copies of the report as well.  If you don&#8217;t see the E-Mail option listed under Delivered By that means your SMTP settings aren&#8217;t setup correctly.