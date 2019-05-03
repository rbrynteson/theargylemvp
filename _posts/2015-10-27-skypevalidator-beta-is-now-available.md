---
id: 1196
title: SkypeValidator beta is now available
date: 2015-10-27T00:51:08+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=1196
permalink: /2015/10/27/skypevalidator-beta-is-now-available/
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
  - Lync Server 2010
  - Lync Server 2013
  - Skype for Business
---
The transition from LyncValidator to SkypeValidator is nearly complete.  Although many items look &#8220;similar&#8221; I can tell you under the hood it&#8217;s a complete rewrite of the code to attempt to make things much faster.  So what are the big changes.

**#1 &#8211; Monitoring Features**

In the previous LyncValidator the monitoring features were pretty much an all or nothing option.  Although we added the option to turn off individual larger categories (like Certs or DNS) this wasn&#8217;t very granular.  So in SkypeValidator, we now offer the ability to enable/disable at each end-point.  Additionally, we allow you to easily add more end-points to your deployment.

To use this new feature, simply go to the Monitoring Tab and Enable Automatic Monitoring.  Once checked, click the Import Monitoring End-Points button and it will add all of your servers automatically.  Additionally, if you make a change to your design, you can click this button again to pick up the new servers.

[<img class="alignnone wp-image-1197 size-large" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/10/1-1024x291.png?resize=800%2C227&#038;ssl=1" alt="1" width="800" height="227" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/10/1.png?resize=1024%2C291&ssl=1 1024w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/10/1.png?resize=300%2C85&ssl=1 300w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/10/1.png?resize=768%2C218&ssl=1 768w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/10/1.png?w=1061&ssl=1 1061w" sizes="(max-width: 800px) 100vw, 800px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/10/1.png)

Once your end-points are imported you then have the option to enable/disable each end-point individually or even delete them.

[<img class="alignnone wp-image-1198 size-large" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/10/2-1024x192.png?resize=800%2C150&#038;ssl=1" alt="2" width="800" height="150" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/10/2.png?resize=1024%2C192&ssl=1 1024w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/10/2.png?resize=300%2C56&ssl=1 300w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/10/2.png?resize=768%2C144&ssl=1 768w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/10/2.png?w=1060&ssl=1 1060w" sizes="(max-width: 800px) 100vw, 800px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/10/2.png)

<span style="color: #ff0000">NOTE: If you are an old user of LyncValidator, now is a good time to import in your end-points.  We will import everyone in the future but to take advantage of the new system you need to import your deployment/end-points.</span>

This change was ABSOLUTELY necessary.  I know it seems like there is an extra step now (and there is) but without it we can&#8217;t scale.  See #4 below.

**#2 &#8211; Availability Reports**

Once you have enabled Automatic Monitoring you might want to know the availability of each service.  On the monitoring page, you will be displayed a bar graph of the availability of each end-points.

[<img class="alignnone size-full wp-image-1199" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/10/3.png?resize=800%2C125&#038;ssl=1" alt="3" width="800" height="125" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/10/3.png?w=971&ssl=1 971w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/10/3.png?resize=300%2C47&ssl=1 300w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/10/3.png?resize=768%2C120&ssl=1 768w" sizes="(max-width: 800px) 100vw, 800px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/10/3.png)

Additionally, on the Monitoring Events page, we will display a roll up of all monitoring events, when they happened and when the resolution occurred.

**#3 &#8211; New HTTP/S Content Check**

Knowing that a web server is listening on port 443 is good but knowing it&#8217;s displaying the correct information is more important.  New to SkypeValidator is the ability to enter a HTTP/S FQDN and a content search term.

[<img class="alignnone size-full wp-image-1200" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/10/4.png?resize=800%2C130&#038;ssl=1" alt="4" width="800" height="130" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/10/4.png?w=975&ssl=1 975w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/10/4.png?resize=300%2C49&ssl=1 300w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/10/4.png?resize=768%2C125&ssl=1 768w" sizes="(max-width: 800px) 100vw, 800px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/10/4.png)

&nbsp;

SkypeValidator will reach out to the website every 60 minutes and make sure that your search term is in the content pulled back from the website.  A great way to validate the Skype for Business Dial-In Page is responding for example.

**#4 &#8211; More Frequent Checks**

Under the old system, we checked for validation every 4 hours by default.  This worked well to start with but with over 100 deployments being validated on a daily basis this caused a backlog.  Our goal of 4 hours was slipping back to nearly 6 hours.

With SkypeValidator, OWAS, Port and HTTP/S Content checks happen every 60 minutes.  DNS Checks happen every 3 hours and certificates are checked every 6 hours (cert checks take forever to complete).  This allows us to process all of the requests MUCH faster.  We will monitor the change and assuming all is good we will pull these numbers down even more.

**#5 &#8211; Easier to Export Data**

When looking at your design validation, you will find that every table can be exported to Excel now.  This is in addition to the Word document you can export as well.

And that is just a small list of changes to SkypeValidator and there is MUCH MORE coming!  So head over to https://skypevalidator.com and see what&#8217;s new.  As always we are looking for feedback so don&#8217;t hesitate in e-mailing me at richard -at- masteringlync.com.