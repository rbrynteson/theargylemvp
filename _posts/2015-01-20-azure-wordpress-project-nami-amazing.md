---
id: 1055
title: Azure + WordPress + Project Nami = Amazing
date: 2015-01-20T03:37:53+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=1055
permalink: /2015/01/20/azure-wordpress-project-nami-amazing/
panels_data:
  - 'a:0:{}'
categories:
  - Azure
---
A little over a week ago I decided it was time for a new mission &#8211; move my blog from my self-hosted, Open SuSE Server running on HyperV to Azure.  The reason for the move was a few items.  First, my blog got a little over 250,000 page views last year and I&#8217;m sure my home Comcast connection wasn&#8217;t really appreciating it.  Second, the site was running on an aging HP Server that is going to see it&#8217;s last day so best to get it somewhere a tad bit more stable.  Lastly, my IIS ARR reverse proxy has been doing some pretty stupid things and it might be time to rebuild it (problems with using home servers as lab + production) and I didn&#8217;t want to take down my blog for days while I fixed it.

So I know that Azure support WordPress + ClearDB for hosting but I didn&#8217;t want to have to pay another provider for MySQL hosting.  Plus, I&#8217;ve been using Azure SQL and I love it and wanted to see if I could find another option.  This is where <a href="http://projectnami.org/" target="_blank">Project Nami</a> comes in.

**Project Nami**

Never heard of them?  I hadn&#8217;t until a few Bing searches either.  Basically Project Nami (Not Another MySQL Interpreter) was created to allow WordPress to talk to Azure SQL natively.  If you know any WordPress development, they make it so $wpdb queries works against Azure SQL natively (there are ALL sorts of amazing implications of this that I will get to at a later time).

**Deploying WordPress to Azure**

The folks at Project Nami have everything hosted in Github which means they even have the ability to deploy to Azure in three simple clicks.  For myself, I have a pretty large Azure deployment including several SQL databases so I wanted to be able to pick some existing servers.  So I followed this video tutorial to get the site up and running:

<span class="embed-youtube" style="text-align:center; display: block;"></span>

Here are the basics (again &#8211; the video has all of this)

  1. Create a new website.  Choose custom create.
  2. Provide a name for your site.  Select your region and choose Create a Free 20 MB SQL Database.  You can leave the connection string.
  3. Give your database a name.  If you have an existing SQL Server, go ahead and pick it or create a new one.  Note your SQL Login name and password.  Lastly, make sure your region is the same as your web server.
  4. Pick External Repository (not github)
  5. On publishing, your repository url is &#8220;https://github.com/ProjectNami/projectnami.git&#8221;.  Your branch to deploy is latest.

At this point in time, sit back and enjoy the build process.  This will take about 5 minutes to complete.  Once the website is deployed.

  1. Go to the WordPress site with your Azure website name.  It will be something like: http://nami.azurewebsites.net/
  2. On the resulting Project Nami page, you need to create your configuration file.  
    [<img class="alignnone wp-image-1057 size-full" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/01/pic1.png?resize=568%2C431&#038;ssl=1" alt="pic1" width="568" height="431" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/01/pic1.png?w=568&ssl=1 568w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/01/pic1.png?resize=300%2C228&ssl=1 300w" sizes="(max-width: 568px) 100vw, 568px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/01/pic1.png)  
    On this page, you will need to enter the database name you created in your Azure Setup.  Your username will include an @ symbol and your host is your host.
  3. Once you have completed this, you will find the normal WordPress configuration page.  I&#8217;m not going to go through the normal WordPress initial configuration &#8211; I&#8217;m assuming if you are moving a WordPress site you know how to set it up.

**WordPress Import/Export**

So now that you have a working WordPress site you need to migrate your old site to this new site.  This process is going to be slightly different per site, I would guess, but here are the high level details.

  1. Export your old site.  Go login to your old WordPress site and choose Tools | Export.  Select everything to export and save the XML file.  It shouldn&#8217;t take too long.
  2. Go to your new sites Dashboard.  Add the WordPress Importer plug-in to your site.  Browse to Tools | Import | WordPress.  When you click on this option it will prompt you to download the plugin.  
    [<img class="alignnone wp-image-1058" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/01/pic02-300x142.png?resize=450%2C213&#038;ssl=1" alt="pic02" width="450" height="213" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/01/pic02.png?resize=300%2C142&ssl=1 300w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/01/pic02.png?resize=768%2C363&ssl=1 768w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/01/pic02.png?w=920&ssl=1 920w" sizes="(max-width: 450px) 100vw, 450px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/01/pic02.png)
  3. Before you import your site, I would suggest that you grab any other plug-ins and themes that you used on your old site for the new site.  This can certainly be a time consuming process but worth it.
  4. Now change your theme to the same theme you had before if you plan on keeping the same look.  If you are going to switch then it doesn&#8217;t really matter as you are going to have to redo your menus, widgets, etc.  If you set the correct theme it appears as though &#8220;most&#8221; of the formatting comes over.
  5. Next you want to import in your site.  Go to Tools | Import | WordPress.  You will be asked to upload your XML file.  Although there is a warning that says there is a 1MB limit, my file was way bigger and imported fine.  When you import into WordPress is will ask if you want to download any images/attachments, say yes.  You will also have the opportunity to reassign your posts to a new user.

Now you need to do some cleanup.  Some things I found along the way that may or may not make your life easier.  First, I would disable JetPack and especially disable any CDN features.  I found that I had to reattach just about every image on my blog but when I did it a second time after JetPack was disabled, it was fine.  Second, prepare for lost features.  I went from WordPress 3.4 all the way to 4.1.  There were a lot of changes during that time frame so I would review the changes.  If you have the ability to update to the latest version it might not be a bad idea.

**Cut Over Time**

When its time to move your website, the biggest item you need to do is change your site URL.  This can be found under Dashboard | Settings | General.  Right now it&#8217;s going to be something http://nami.azurewebsites.net and we are going to change that to http://yourdomain.com.  Once you hit submit, your site will not be reachable or it will send you off to your old site.

I would suggest editing your local computer HOSTS file for your domain so you are going to Azure instead.  This would be the time to make sure everything is working correctly.  Once it all looks good &#8211; time to change your DNS records.

**Caching**

The good folks over at Project Nami have created a caching plug-in that using Azure Blob files.  This prevents a bunch of PHP page loads and makes the site even faster.  You can find the plug-in here:

<div class="fitvids-video">
  <blockquote data-secret="dyFOnI864u" class="wp-embedded-content">
    <p>
      <a href="http://projectnami.org/project-nami-blob-cache-build-2-now-available/">Project Nami Blob Cache (Build 2) now available</a>
    </p>
  </blockquote>
  
  <p>
    </div> 
    
    <p>
      Make sure to check out the readme file for details.
    </p>
    
    <p>
      NOTE: I turned this on last night and my memory utilization in Azure went through the roof and caused me to exceed my allotment and wait for the hourly reset.  I&#8217;m going to play with this more and see if this was the cause or if I caused it some other way.
    </p>
    
    <p>
      <strong>Multisite Deployments</strong>
    </p>
    
    <p>
      Does Nami support them?  Yep, my blog is a multisite deployment.  The only strange thing I ran into &#8211; activating the PN Cache plug-in crashed the site if I Network Activated it.  I think it has something to do with themes but I didn&#8217;t care to find out.
    </p>
    
    <p>
      That is it.  As I said, the good folks at Project Nami have made this dirt simple to setup/install.
    </p>
    
    <p>
      &nbsp;
    </p>
    
    <p>
      &nbsp;
    </p>