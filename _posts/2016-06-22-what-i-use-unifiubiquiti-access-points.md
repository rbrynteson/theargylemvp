---
id: 1266
title: 'What I Use: UniFi/Ubiquiti Access Points'
date: 2016-06-22T14:46:01+00:00
author: Richard Brynteson
layout: post
guid: http://masteringlync.com/?p=1266
permalink: /2016/06/22/what-i-use-unifiubiquiti-access-points/
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
  - Uncategorized
---
Every once in a while I get asked what do I use at home/small office and today I figured what a great opportunity to blog about home WiFi.  So this is a new post in my What I Use series.  Before continuing, it should be noted that I purchased all of the equipment I&#8217;m about to talk about and Ubiquiti Networks didn&#8217;t give me a deal, didn&#8217;t ask me to blog or anything else.  i.e. all content are my own thoughts.

**What I Used Before**

Like so many in the home office I was simply using what was provided to me by my internet provider &#8211; in this case Comcast.  So I had started with the Comcast Modem, was paying my $9.95 per month and using their wireless access point built into the device.  Needless to say, it wasn&#8217;t a great experience.  Coverage was spotty, I was paying for a modem and randomly throughout the day the modem would drop down to 250k and only a reboot would fix it.

So about three months ago, I purchased my own cable modem and used an old NetGear wireless access point.  This solved the random speed drops to 250k but wireless was still spotty and got significantly worse when the kids (and their friends) would come over.  At times we had 25+ devices on the WiFi.

**New Solution**

<a href="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/06/1.png" rel="attachment wp-att-1269"><img class="alignnone size-full wp-image-1269" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/06/1.png?resize=713%2C446&#038;ssl=1" alt="1" width="713" height="446" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/06/1.png?w=713&ssl=1 713w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/06/1.png?resize=300%2C188&ssl=1 300w" sizes="(max-width: 713px) 100vw, 713px" data-recalc-dims="1" /></a>

The solution is two AP-AC-Pro access points and the Unifi Security Gateway.  Additionally, to control all of this you need a server/workstation/something to act as a controller for your environment.  In my case, I&#8217;m going to use my home windows server for this (NOTE: Since using the Windows box I&#8217;ve discovered there are a few things the controller won&#8217;t do for me &#8211; maybe Linux would have been better.)

**Setup of Devices**

The setup starts with the install of the controller.  Needless to say, the install is a quick &#8220;next-next-next&#8221; type of installer.  There aren&#8217;t any difficult questions to figure out or do.  There are a couple of pre-reqs:

  * Java 7 or later
  * Port 8080 and 8443 must be open/available

When the installer is done, I would recommend moving the folder.  By default, it gets installed to &#8220;%windir%\users\%username%\Ubiquiti UniFi&#8221; which is a silly place to install anything.  The good news is, simply move the folder to a new location.  I would recommend &#8220;%windir%\Program Files\Ubiquiti UniFi&#8221; as a location.

Once you have install the product, you can launch the controller software.  A shortcut would have been placed on your desktop (we will talk installing as a service later) &#8211; if you moved the install folder make sure to update your shortcut.  Once the software is running, browse to https://localhost:8443 to start the process.

**Setup and Config**

When you visit the site for the first time it will display the devices it found on the network.  In my case, two AP-AC-Pro access points.  I added the gateway after I had the WiFi up and running.

<a href="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/06/2.png" rel="attachment wp-att-1270"><img class="alignnone size-full wp-image-1270" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/06/2.png?resize=683%2C250&#038;ssl=1" alt="2" width="683" height="250" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/06/2.png?w=683&ssl=1 683w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/06/2.png?resize=300%2C110&ssl=1 300w" sizes="(max-width: 683px) 100vw, 683px" data-recalc-dims="1" /></a>

Enter the SSID, Password and choose if Guest Access will be enabled.

<a href="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/06/3.png" rel="attachment wp-att-1271"><img class="alignnone size-full wp-image-1271" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/06/3.png?resize=656%2C346&#038;ssl=1" alt="3" width="656" height="346" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/06/3.png?w=656&ssl=1 656w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/06/3.png?resize=300%2C158&ssl=1 300w" sizes="(max-width: 656px) 100vw, 656px" data-recalc-dims="1" /></a>

Next, enter your username and password for the controller.  It&#8217;s good to note that this combo will also be the SSH/Console credentials needed for each individual device.

<a href="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/06/4.png" rel="attachment wp-att-1272"><img class="alignnone size-full wp-image-1272" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/06/4.png?resize=676%2C250&#038;ssl=1" alt="4" width="676" height="250" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/06/4.png?w=676&ssl=1 676w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/06/4.png?resize=300%2C111&ssl=1 300w" sizes="(max-width: 676px) 100vw, 676px" data-recalc-dims="1" /></a>

Click Next and it shows a summary of settings and you are done.  The controller will now configure the access points to how you want them.

**Daily Use**

Once your controller is setup you will see the following dashboard.  This allows you to see the currently connected device, daily and hourly throughput and latency on the network.  Additionally, you can configure advanced items such as periodic speed tests and more.

<a href="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/06/5.png" rel="attachment wp-att-1273"><img class="alignnone size-medium wp-image-1273" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/06/5-300x143.png?resize=300%2C143&#038;ssl=1" alt="5" width="300" height="143" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/06/5.png?resize=300%2C143&ssl=1 300w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/06/5.png?resize=768%2C365&ssl=1 768w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/06/5.png?resize=1024%2C486&ssl=1 1024w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/06/5.png?w=1341&ssl=1 1341w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" /></a>

**Adding New Equipment**

As you add new equipment to your UniFi network it will appear under the devices tab.

<a href="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/06/6.png" rel="attachment wp-att-1274"><img class="alignnone size-medium wp-image-1274" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/06/6-300x55.png?resize=300%2C55&#038;ssl=1" alt="6" width="300" height="55" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/06/6.png?resize=300%2C55&ssl=1 300w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/06/6.png?resize=768%2C141&ssl=1 768w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/06/6.png?resize=1024%2C188&ssl=1 1024w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/06/6.png?w=1325&ssl=1 1325w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" /></a>

As those devices are discovered, you simply click the &#8220;Adopt&#8221; button on the far right side and the controller will take control of the device.  It should be noted that any already configured devices do not appear in this list so a factory reset might would be required if you setup these devices previously.

**Zero Handoff or RSSI**

One feature that many high-end controllers have is zero handoff.  This allows a computer to seamlessly connect to another WiFi access point as you travel around your building.  This feature relies on the idea that all access points in your deployment use the same channel for wireless.  This might work really well in an office building or corporate location where density is low but at home, having to force all devices to a specific channel, may not be ideal.  As I write this post, I see 9 different WiFi access points being advertised.  I want my controller to move the channels around whenever it wants because my neighbors seem to add new SSID&#8217;s on a daily basis for some reason.  Lastly, for me, I don&#8217;t randomly walk around with my laptop throughout the house.  And I NEVER use the Skype for Business Mobile client (mainly because the Windows Phone version is terrible).  So the zero handoff is really not needed for me.

But I have another concern.  My basement (where one of the access points was placed) had terrible performance in the past.

So in the end, I decided to use a Minimum RSSI setting.  What this will do is kick the device off of the access point and force a reconnect if I travel too far away (i.e. signal gets too low).  The idea is that my access point in the basement will only serve the devices in the basement and as I move to the higher levels in the house the signal would be too weak, kick me off, and then use the upstairs access point instead.

I currently have my Minimum RSSI set to a -75 dBm in the Controller.

NOTE: This setting used to be done via a config file only and the math/name was different previously.  So before you may have set a minrssi of 20 &#8211; per this <a href="https://help.ubnt.com/hc/en-us/articles/205144650-UniFi-Set-minimum-RSSI-for-clients" target="_blank">guide</a>.  Based on what I&#8217;ve found, the naming is a little goofy so a -75 is actually a value of 20 that is listed in the guide.  Essentially, take -95 + -75 (your value) to get what the old config file would have done.

<a href="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/06/7.png" rel="attachment wp-att-1275"><img class="alignnone size-medium wp-image-1275" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/06/7-300x291.png?resize=300%2C291&#038;ssl=1" alt="7" width="300" height="291" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/06/7.png?resize=300%2C291&ssl=1 300w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/06/7.png?w=380&ssl=1 380w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" /></a>

Thus far in my testing, this has worked exactly as I expected devices have connected where I wanted them to connect.  As I walk up the stairs from my basement, my device disconnected and reconnected to the other access point.  In all, it takes about 1 to 2 seconds to handoff between devices.  And again, I don&#8217;t walk around with my laptop open so I&#8217;m in good shape.

**Run As A Service**

As I mentioned before, if you installed the controller software on a windows server/workstation it is not setup to run as a service by default.

Ubiquiti has a document <a href="https://help.ubnt.com/hc/en-us/articles/205144550-UniFi-Run-the-controller-as-a-Windows-service" target="_blank">here </a>on how to configure the controller software as a service.  The installer should really do this for you but the configuration is pretty easy.

**Performance**

So I&#8217;ve been using my Ubiquiti equipment now for the last two weeks and I have to say the performance is outstanding.  I no longer get complaints from the family that the wifi is down, the internet is slow, etc.  And my SfB calls no longer go red in every single call like before.

My next purchase (if I can ever find a store with them available) will be two 8 port Ubiquiti switches.  These devices support PoE, QoS, etc.  And they aren&#8217;t very expensive based on what I&#8217;ve seen online.

**Outstanding Issues**

The only item that doesn&#8217;t work on my network anymore is my SpectraLink 8440 test device.  I&#8217;ve done the research and it appears as though the AC-AP-Pro do not have WME enabled by default.  This isn&#8217;t set in the Controller GUI so I&#8217;ll need to push a config file to the devices to enable this.  Maybe a future post but I never used the device except for a few test calls.

&nbsp;