---
id: 1506
title: Recover a VVX Phone after a failed upgrade
date: 2018-05-08T15:56:46+00:00
author: Richard Brynteson
layout: post
guid: http://masteringlync.com/?p=1506
permalink: /2018/05/08/recover-a-vvx-phone-after-a-failed-upgrade/
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
image: https://masteringlync.com/wp-content/uploads/sites/2/2018/05/VVX500-1-large.jpg
categories:
  - Lync Server 2010
  - Lync Server 2013
  - Skype for Business
---
So no one can tell me why this has happened yet and it appears as though the problem has been present for several weeks and nothing from Polycom but there is a problem with the 5.7.1 CAB files that are posted on their website.

<img class="alignnone size-full wp-image-1507" src="https://masteringlync.com/wp-content/uploads/2018/05/1.png?resize=619%2C366&#038;ssl=1" alt="" width="619" height="366"  /> 

When you download these files and import them into your Skype for Business or Lync Server you will discover that 5.7.1.1698 did NOT get imported but rather 5.7.1.2205 is instead.  Worst though is that when the phones attempts to update it will go through the initial &#8220;Updater&#8221; process without any issues but then will go into a reboot loop when it tries to install the firmware complaining it cannot find the sip.ld file.  My testing found this doesn&#8217;t happen to all phones but certainly is 100% reproducible on the VVX 500 and VVX410 devices.  My guess it is on others as well and I cannot tell you why the some phones work and others don&#8217;t.

So what should you do once Polycom has crippled your phone? Well, it&#8217;s going to take a bit of work but here goes:

#1 &#8211; Download the [Split] files from above.  I would recommend the same version that is broken.  You will need to install an FTP Server somewhere.  A local machine will do and have those files be accessible at the root.  Anonymous access won&#8217;t work and I would just recommend using the default user/password.

If you need more details about it all, go take a peak at <a href="http://blog.schertz.name/2013/05/provisioning-polycom-sip-phones/" target="_blank" rel="noopener">Jeff&#8217;s blog</a>.  When everything is done you should be able to open up your faviorite browser go to ftp://ipaddress/ and it should prompt for a username/password.  Use the default PlcmSpIp and you should see the director.  You are going to extract and put everything in the SPLIT file as the default.

#2 &#8211; On the phone that is doing the Polycom Reboot Death Dance (PRDD) click on Setup when you see it.

<img class="alignnone size-full wp-image-1508" src="https://masteringlync.com/wp-content/uploads/sites/2/2018/05/2.png?resize=467%2C346&#038;ssl=1" alt="" width="467" height="346" /> 

#3 &#8211; End in the Administrator password.  You HAVE to know the password for this to work.  The default password is 456.

#4 &#8211; Select Provisioning.  If the phone is a VVX 400/300/200 model you can use the up/down arrows to select.  If using a VVX 600/500 then you can use the head phone, speaker and + or – to move around.  The mute button is OK.

<img class="alignnone size-full wp-image-1509" src="https://masteringlync.com/wp-content/uploads/2018/05/3.png?resize=482%2C361&#038;ssl=1" alt="" width="482" height="361" /> 

#5 -Go down to **Upgrade Server** and select Edit.  Using the back button delete everything here.

<img class="alignnone size-full wp-image-1510" src="https://masteringlync.com/wp-content/uploads/2018/05/4.png?resize=497%2C384&#038;ssl=1" alt="" width="497" height="384" /> 

#6 &#8211; Go to Server Type and choose FTP or FTPS depending on your setup.  Then under Server Address enter the IP or FQDN of your server.  In my case I have an FTPS server that I used for this process.

<img class="alignnone size-full wp-image-1514" src="https://masteringlync.com/wp-content/uploads/2018/05/5-1.png?resize=407%2C311&#038;ssl=1" alt="" width="407" height="311" /> 

&nbsp;

#7 – Click on Exit twice.  You will then be asked if you want to Save Config.

<img class="alignnone size-full wp-image-1512" src="https://masteringlync.com/wp-content/uploads/2018/05/6.png?resize=391%2C301&#038;ssl=1" alt="" width="391" height="301"  /> 

#8 – The phone will reboot and upgrade the phone and complete the 5.7.1 upgrade.  After the phone finishes rebooting, go to the Web GUI or via the Phone Menu.  Go Settings | Provisioning.  You will see the Server Address listed before.  Remove the Server Address to make it blank.  Click Save.

<img class="alignnone size-full wp-image-1516" src="https://masteringlync.com/wp-content/uploads/2018/05/1-1.png?resize=366%2C309&#038;ssl=1" alt="" width="366" height="309" /> 

And there you go.  I&#8217;ll update if I ever hear anything back from Polycom as to the root issue or why this update is doing the PRDD.