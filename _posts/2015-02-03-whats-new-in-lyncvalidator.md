---
id: 1066
title: 'What&#8217;s New in LyncValidator'
date: 2015-02-03T04:43:46+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=1066
permalink: /2015/02/03/whats-new-in-lyncvalidator/
panels_data:
  - 'a:0:{}'
categories:
  - Lync Server 2013
---
I try not to fill my blog with updates of the LyncValidator but occasionally the changes are enough to deem an explanation or a quick guide on how to use them &#8211; this is one of those posts.

**Service Monitoring**

The goal all along was to build more than a tool that simply spits out DNS and firewall records but to build something that could be used at the start, middle and end of your Lync adventure.  I think we have done a pretty good job of covering the start (I promise Mediation Server and Branch Offices are coming) but now to focus on the last two parts.

With that in mind we are expanding the feature set of Lync Monitoring.  After you have built your deployment, you can click on the monitoring tab and choose which parts of your deployment you want to automatically check.  If something goes offline, the firewall team closes a port, whatever scenario causes a loss of services &#8211; LyncValidator will e-mail you a summary report that something is down.

[<img class="alignnone wp-image-1072 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/02/1a.png?resize=600%2C154&#038;ssl=1" alt="1a" width="600" height="154" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/02/1a.png?w=600&ssl=1 600w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/02/1a.png?resize=300%2C77&ssl=1 300w" sizes="(max-width: 600px) 100vw, 600px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/02/1a.png)

Now in addition to that, we have added custom monitoring options.  Here you can choose items to monitor outside of the pre-defined items within Lync.  Maybe you want to monitor your Exchange Web Services or another web service of your choosing, you can choose between Port (TCP/UDP), DNS, Certificate and Office Web App Server availability.  We will be adding more options in the future.  Also, I&#8217;ll be working on making this feature easier to use for those who don&#8217;t have a Lync Deployment but something else to monitor.

[<img class="alignnone wp-image-1068 size-column2-1/1" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/02/2-570x490.png?resize=570%2C490&#038;ssl=1" alt="2" width="570" height="490" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/02/2.png)

**Deployment Sharing**

One of the features that is often requested is the ability to share a deployment with other members of your team.  The reasons for this are many.  You could be a partner helping a customer with a deployment.  You could work on a large team.  Whatever the case, we have a solution for you.  This is our first attempt and I have already gotten some great feedback and have a few other items planned around this feature already.

To use this feature, click on the Dashboard Page.  Here you will see the current status of your deployment, a list of any individuals that your deployment may be shared with and the full results of your last monitoring report.

[<img class="alignnone wp-image-1073 size-full" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/02/3.png?resize=596%2C354&#038;ssl=1" alt="3" width="596" height="354" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/02/3.png?w=596&ssl=1 596w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/02/3.png?resize=300%2C178&ssl=1 300w" sizes="(max-width: 596px) 100vw, 596px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/02/3.png)

On the sharing page enter the email address of the person you want to share with and click Share.  The users e-mail MUST be a user already within the system.

**Encryption + SSL**

Another piece of feedback I get all the time is &#8220;what are you doing with this data&#8221;.  I&#8217;ve said from the start I&#8217;m not doing anything with the information and I&#8217;m taking steps to make it harder for anyone to even see it.  Next time you login to the Lync Validator it&#8217;s going to encrypt your information in the Azure SQL Database so on the off chance someone connected via Access you can&#8217;t get any useful information.

Additionally, just to make sure no one else can either, our site is SSL based now.  If you had an old link to the site, don&#8217;t worry we will redirect you to the correct place.

**Other Changes**

Here are a few other changes:

  * Changed how session variables are handled so the web page won&#8217;t time out after 30 minutes.
  * Bug fixes related to topology imports, including importing single IP edge servers.  (Thanks Pat for letting us know!)
  * Updated the HOSTS file generation to include the internal edge services.

**Free vs Donation Features**

In an effort to draw a clear line in the sand between community tool and &#8220;not make me broke tool&#8221; here is how I see the breakdown of features.

<div>
  <table border="1" cellspacing="0" cellpadding="0">
    <tr>
      <td>
        Features
      </td>
      
      <td>
        Free
      </td>
      
      <td>
        Suggested Donation ($10/year)
      </td>
    </tr>
    
    <tr>
      <td>
        Deployments
      </td>
      
      <td>
        Unlimited
      </td>
      
      <td>
        Unlimited
      </td>
    </tr>
    
    <tr>
      <td>
        Export Documentation
      </td>
      
      <td>
        Included
      </td>
      
      <td>
        Included
      </td>
    </tr>
    
    <tr>
      <td>
        Import Topologies
      </td>
      
      <td>
        Included
      </td>
      
      <td>
        Included
      </td>
    </tr>
    
    <tr>
      <td>
        Design Validation
      </td>
      
      <td>
        Included
      </td>
      
      <td>
        Included
      </td>
    </tr>
    
    <tr>
      <td>
        Monitoring
      </td>
      
      <td>
        One
      </td>
      
      <td>
        Up to 10 Sites
      </td>
    </tr>
    
    <tr>
      <td>
        Service Monitoring Features
      </td>
      
      <td>
        Up to 10 individual items
      </td>
      
      <td>
        Up to 100 Individual Items
      </td>
    </tr>
    
    <tr>
      <td>
        Shared Deployments
      </td>
      
      <td>
        No
      </td>
      
      <td>
        Yes
      </td>
    </tr>
  </table>
</div>

Hopefully the above seems fair to everyone.

&nbsp;

&nbsp;