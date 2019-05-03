---
id: 1579
title: WordPress Multisite + LetsEncrypt
date: 2018-07-23T10:41:19+00:00
author: Richard Brynteson
layout: post
guid: http://masteringlync.com/?p=1579
permalink: /2018/07/23/wordpress-multisite-letsencrypt/
colormag_page_layout:
  - default_layout
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
image: https://masteringlync.com/wp-content/uploads/sites/2/2018/07/1-3.png
categories:
  - Azure
  - Development
  - What I Use
---
As of the end of this month, [Chrome 68](https://security.googleblog.com/2018/02/a-secure-web-is-here-to-stay.html) will start marking all non-SSL sites as non-secure.  Additionally, it has been well reported that Google and Microsoft prefer SSL sites over non-SSL sites in their search results.  So as a blogger and user of WordPress I decided it was high time to get it done.  So I started my research a while ago to see what options are out there.  I could use a traditional SSL Certificate Provider like DigiCert (they are THE BEST!) but I&#8217;m not really experienced with Linux and SSL and honestly scared a bit.  So I decided to go for the LetsEncrypt route.

**Who is LetsEncrypt?**

Let’s Encrypt is a free, automated, and open certificate authority (CA), run for the public’s benefit. It is a service provided by the [Internet Security Research Group (ISRG)](https://letsencrypt.org/isrg/).  You can read all about them on their [website](https://letsencrypt.org/about/).  Essentially, they exist to give websites (and other services) access to free SSL certs so everything is secure.

**What&#8217;s My Setup?**

Several years ago I moved my blog from a locally hosted server to Azure.  I tried a few different Azure related services but honestly decided that a CentOS box worked best.  It has the best performance, low maintenance and it just worked.

**Let&#8217;s Add A Cert!**

So the process for adding the certificate to your existing Apache deployment wasn&#8217;t too hard.  There were a few bumps along the road so hopefully this will help people out.  This assumes you have Apache and WordPress already installed.

**Step #1**: Install the software

Before we can install certbot (which is the preferred client for LetsEncrypt) you just need to make sure you have a few things installed.  First, enable the EPEL library:

<pre>$ sudo yum install epel-release</pre>

Now, we can add mod_ssl and the certbot to apache:

<pre>$ sudo yum install httpd mod_ssl python-certbot-apache</pre>

**Step #2**: Request SSL Certificate from Let&#8217;s Encrypt.

To request a certificate, we are going to use the sudo certbot &#8211;apache command.  That command also includes a -d which is used to define the domain or domains you are going to use.  The great thing about this is you can add as many domains as you want to your certificate.  So for example, maybe you have the following:

admin.domaina.com (WordPress Multi-Site Domain)  
masteringlync.com  
stolenwebsitefromlasko.com

So my command would be:

<pre>$ sudo certbot --apache -d admin.domaina.com -d masteringlync.com -d www.masteringlync.com -d stolenwebsitefromlasko.com -d www.stolenwebsitefromlasko.com</pre>

Now here is the critical part for WordPress Multi-Site users.  The above command is going to fail right now.  What CertBot will do is check your Apache configuration and see if you have those domains defined in your virtual directories.  However, if you have ever used WordPress Multi-Site previously you know you have NEVER had to modify that document before.  Well, now you will.

So head over to /etc/httpd/conf and modify your httpd.conf file.  So:

<pre>$ cd /etc/httpd/conf
$ sudo vi httpd.conf</pre>

You are going to head to the bottom of this configuration file and add the following for each domain you have listed above:

<pre>&lt;VirtualHost *:80&gt; 
    ServerAdmin admin@example.com
    ServerName masteringlync.com
    ServerAlias www.masteringlync.com
    DocumentRoot /var/www/html 
&lt;/VirtualHost&gt;</pre>

Now, since you are pointing your VirtualHost for each of these to the DocumentRoot folder for the main website this isn&#8217;t going to break your Apache + WordPress configuration.  But what it will do is allow you to run the above certbot command.  Go ahead and run that now.

You will be prompted about if you want to forward all http to https traffic.  In my case, I didn&#8217;t want to do this because I had no idea if WordPress was going to freak out, so I choose #1.

<img class="alignnone size-full wp-image-1580" src="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2018/07/2-3.png?resize=644%2C163&#038;ssl=1" alt="" width="644" height="163" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2018/07/2-3.png?w=644&ssl=1 644w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2018/07/2-3.png?resize=300%2C76&ssl=1 300w" sizes="(max-width: 644px) 100vw, 644px" data-recalc-dims="1" /> 

After selecting #1 you should see this:

<img class="alignnone size-full wp-image-1581" src="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2018/07/1-3.png?resize=640%2C225&#038;ssl=1" alt="" width="640" height="225" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2018/07/1-3.png?w=640&ssl=1 640w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2018/07/1-3.png?resize=300%2C105&ssl=1 300w" sizes="(max-width: 640px) 100vw, 640px" data-recalc-dims="1" /> 

**Step #3:** Test

Now your SSL should be work.  Head over to your favorite browser and make sure HTTPS is working.  If it is, success.

**Step #4**: Force SSL

So now that you have SSL working, you want to force SSL on website.  You could go back and play with the Apache Configuration but I don&#8217;t want to try to understand that so I&#8217;m going with plugins.  I used two of them:

#1 &#8211; WP Force SSL.  This does exactly what you think it would, takes all http traffic and sends it over to https.  This works for about 95% of your content.  But maybe you added a media file directly and used http in the name.

#2 &#8211; SSL Insecure Content Fixer.  This plugin will find any of your pesky items and update it on the fly when it&#8217;s requested.  Pretty cool.

**Step #5**: Renewal Time

So the one item to know about Let&#8217;s Encrypt is that all certs are good for only 90 days.  So you need to renew them.  You can do that easily with:

<pre>$ sudo certbot renew</pre>

But maybe you don&#8217;t want to set a calendar reminder to update all the time.  So that is what cron is for.  Run these:

<pre>$ sudo crontab -e
00 1 * * * /usr/bin/certbot renew &gt;&gt; /var/log/letsencrypt.log</pre>

This will run the renewal process, everyday at 1AM local time for the server.  And it dumps the results to a log file.  The cool thing about the certbot renew command is that if the cert is not less than 30 days from expiring, it simply will do nothing.

<img class="alignnone size-full wp-image-1582" src="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2018/07/3-2.png?resize=644%2C256&#038;ssl=1" alt="" width="644" height="256" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2018/07/3-2.png?w=644&ssl=1 644w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2018/07/3-2.png?resize=300%2C119&ssl=1 300w" sizes="(max-width: 644px) 100vw, 644px" data-recalc-dims="1" /> 

That&#8217;s it.  Your WordPress + MultiSite deployment has SSL Certs for all the website.  Hopefully that helps someone else.

&nbsp;

&nbsp;