---
id: 880
title: Lync Validator Adds Automatic System Monitoring
date: 2014-10-31T02:34:28+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=880
permalink: /2014/10/31/lync-validator-adds-automatic-system-monitoring/
st_layout_box:
  - right
panels_data:
  - 'a:0:{}'
categories:
  - Lync Server 2013
---
One of the often requested features of the Lync Validator is some sort of automatic system monitoring.  So in this months update we are going to introduce continuous monitoring of your Lync deployment.

**How Does It Work?**

LyncValidator currently checks external services for Certificate Validation, Edge and Reverse Proxy Ports are available, DNS Records and Office Web App services are available.

All deployments that are configured for automatic checking (this is called Premium Feature throughout the application) are checked on an hourly basis.  If there are no errors found during this check, the site is marked as UP and last results are saved.  If there are any errors round in the deployment, the site will be marked as in ALERT status.

Any site that is marked in ALERT status (or DOWN status) are checked every five minutes for a change.  If a site is found to be up, the site will be marked as UP again and placed back into the hourly check cycle.  If a site is found with an error for four consecutive checks, it will be changed from ALERT to DOWN.  Once a site is marked as DOWN an e-mail will be generated and sent to your Microsoft Account.  E-mails will continue to be sent every five minutes until the deployment becomes available again or the site is marked in suspended mode.  When a site goes from DOWN to UP, an e-mail is generated notifying you that the site is in an UP state.

All of the monitoring is being done via Windows Azure.  So the current reliability of system alerts and monitoring checks are based on the availability of Windows Azure.  I&#8217;m basically saying 99.9% as that is what Microsoft offers.

**How To Enable This?**

<span style="text-decoration: underline">Enable Premium Features</span>

Login to the LyncValidator site.  On the home page, you should see an option for User Preferences.

[<img class="alignnone wp-image-881 size-full" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/10/pic12.png?resize=800%2C162&#038;ssl=1" alt="pic1" width="800" height="162" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/10/pic12.png?w=891&ssl=1 891w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/10/pic12.png?resize=300%2C61&ssl=1 300w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/10/pic12.png?resize=768%2C155&ssl=1 768w" sizes="(max-width: 800px) 100vw, 800px" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.com/files/2014/10/pic12.png)

On the preference page, you have several options.

[<img class="alignnone wp-image-882 size-full" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/10/pic21.png?resize=546%2C288&#038;ssl=1" alt="pic2" width="546" height="288" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/10/pic21.png?w=546&ssl=1 546w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/10/pic21.png?resize=300%2C158&ssl=1 300w" sizes="(max-width: 546px) 100vw, 546px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.com/files/2014/10/pic21.png)

**E-Mail Address:** The E-Mail address currently isn&#8217;t used but we will be using it in the future for update e-mails.

**Send E-mail Updates:** In the future we will send updates for new features, any planned outages, etc.

**Premium Account:** This feature needs to be enabled to use the continuous monitoring feature.  Feel free to check that box now.

**Premium Licenses:** This is the number of deployments you are allowed to enable for continuous features.  Everyone is limited to one deployment.

Make sure to click Save Changes.

<span style="text-decoration: underline">Verify Your Site</span>

Before you enable automatic monitoring it would be wise to ensure that your site is going to come back as UP otherwise you are asking to have hundreds of e-mail sent to you.  Go to your deployment and choose Validate Deployment.  On that page, click Run Validation Process.  This will verify all aspects of your deployment.  If anything comes back as failed you should resolve those issues before continuing.

Once your deployment comes back clean move to the next step.

<span style="text-decoration: underline">Enable Automatic Monitoring</span>

Next, we need to enable monitoring on a particular deployment.  Start by editing your deployment.  On the left hand menu, you should now see an option for Premium Feature.

[<img class="alignnone wp-image-883 size-full" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/10/pic3.png?resize=193%2C421&#038;ssl=1" alt="pic3" width="193" height="421" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/10/pic3.png?w=193&ssl=1 193w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/10/pic3.png?resize=138%2C300&ssl=1 138w" sizes="(max-width: 193px) 100vw, 193px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.com/files/2014/10/pic3.png)

When you click on the Premium Features page, you should have a single option today.

[<img class="alignnone wp-image-884 size-full" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/10/pic4.png?resize=575%2C247&#038;ssl=1" alt="pic4" width="575" height="247" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/10/pic4.png?w=575&ssl=1 575w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/10/pic4.png?resize=300%2C129&ssl=1 300w" sizes="(max-width: 575px) 100vw, 575px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.com/files/2014/10/pic4.png)

All you need to do is enable automatic monitoring.  Remember, by default everyone has the ability to monitor a single deployment.

<span style="text-decoration: underline">How To Check It?</span>

When you have enabled Automatic Monitoring when you view your list of existing deployments, you will see a two different visual indicators it&#8217;s been enabled.

[<img class="alignnone wp-image-886 size-large" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/10/pic5-1024x361.png?resize=800%2C282&#038;ssl=1" alt="pic5" width="800" height="282" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/10/pic5.png?resize=1024%2C361&ssl=1 1024w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/10/pic5.png?resize=300%2C106&ssl=1 300w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/10/pic5.png?resize=768%2C271&ssl=1 768w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/10/pic5.png?w=1191&ssl=1 1191w" sizes="(max-width: 800px) 100vw, 800px" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.com/files/2014/10/pic5.png)

At the top of the screen, you will see a tile for your deployment.  It will visually give you the current state of your deployment.  In my example, it&#8217;s red and DOWN.  If you hover over the tile, it will show you the date and time of your last validation check.  Additionally, all sites that are enabled for automatic monitoring will be listed in bold.

Clicking on the tile will bring you to the dashboard for the site.

[<img class="alignnone wp-image-887 size-full" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/10/pic6.png?resize=800%2C388&#038;ssl=1" alt="pic6" width="800" height="388" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/10/pic6.png?w=806&ssl=1 806w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/10/pic6.png?resize=300%2C146&ssl=1 300w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/10/pic6.png?resize=768%2C373&ssl=1 768w" sizes="(max-width: 800px) 100vw, 800px" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.com/files/2014/10/pic6.png)

Here, we again have a tile showing the current status.  Next to that is the ability to suspend checks for your site for a period of time in the blue tile.  You can enter any amount of time in minutes into the provided text box.  When a deployment is marked in suspension, it will go from red to yellow.

<span style="text-decoration: underline">Is This Free?</span>

As of today, everyone is able to use a single site for free.  I reserve the right to change this whenever I want (or turn it off for everyone).  Windows Azure isn&#8217;t free to use and I&#8217;m not looking to get rich off of this either.  If you find this useful, on the user preference page is a donation button for a total of $10.  Why $10?  Because based on what I&#8217;ve estimated for the cost to run the automatic checking, it&#8217;s going to cost about that much on an annual basis to check a site.  This is my best guess estimate.  I also reserve the right to change my mind in the future.  If you donate $10, I&#8217;ll ensure your deployment doesn&#8217;t get turned off for the year.  If you need to monitor more than one deployment, you should e-mail me at <richard@masteringlync.com> and we can chat.

<span style="text-decoration: underline">New Features</span>

The plan is to add more features.  The first thing we are going to add is the ability to choose which checks to alert on.  Today&#8217;s its an all or nothing situation.  We will offer checks based on each category (certs, OWAS, DNS, etc.).  If you have other ideas to make the product better please let me know with my e-mail above.