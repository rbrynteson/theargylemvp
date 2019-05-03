---
id: 1334
title: SkypeValidator Next Gen
date: 2016-11-04T21:31:43+00:00
author: Richard Brynteson
layout: post
guid: http://masteringlync.com/?p=1334
permalink: /2016/11/04/skypevalidator-next-gen/
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
colormag_page_layout:
  - default_layout
categories:
  - Skypevalidator
---
It&#8217;s time to talk about next generation of the Skype Validator.  My goal is each year have a major update to the product and this year is no different.  This year the goal is to address three primary items:

  * Improve navigation.  This includes making it easier to use other features without having to go through the entire design model.
  * Improve the Phone Provisioning processes.  Last year we introduced a very basic Polycom VVX provisioning server.  It was based on HTTPS, allowed you to generate files and you could even point phones to it.  But it lacked flexibility, logs viewing, etc.
  * Improve Monitoring Tools.  Current system creates way too many false positives, too many e-mails get lost in spam and it&#8217;s just not easy to setup.

At this time I&#8217;m ready to talk about item #1 and #2.  We are shooting for end of year for the Next Gen release to everyone.  I&#8217;ll review the new monitoring items in the future.

**Improved Navigation**

One of the biggest issues with SkypeValidator is that you essentially were forced to enter all of your information to get to monitoring or VVX config.  And if you had all of this information already entered into the system, clicking on your deployment made you click a bunch of times before you got to what you wanted.

With the new update when creating a new deployment you will see the following:

<a href="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/11/1-2.png" rel="attachment wp-att-1335"><img class="alignnone wp-image-1335 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/11/1-2.png?resize=632%2C398&#038;ssl=1" alt="1" width="632" height="398" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/11/1-2.png?w=632&ssl=1 632w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/11/1-2.png?resize=300%2C189&ssl=1 300w" sizes="(max-width: 632px) 100vw, 632px" data-recalc-dims="1" /></a>

(This is a mock up &#8211; not final icons by any means.)

Clicking on Design will be able to send to the traditional design tools.  Monitoring will default to the monitoring tools.  And Phone Provisioning will bring you to the VVX phone setup.

In addition to this, once you are in the deployment, you will see a full menu of options and no longer need to step through each item.

<a href="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/11/2-2.png" rel="attachment wp-att-1336"><img class="alignnone wp-image-1336 size-full" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/11/2-2.png?resize=211%2C478&#038;ssl=1" alt="2" width="211" height="478" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/11/2-2.png?w=211&ssl=1 211w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/11/2-2.png?resize=132%2C300&ssl=1 132w" sizes="(max-width: 211px) 100vw, 211px" data-recalc-dims="1" /></a>

**Phone Provisioning**

The existing tools for phone provisioning worked but the goal was to make it more robust in nature.  So let&#8217;s look at some changes.

<span style="text-decoration: underline">Introduction of Locations</span>

At the top most level you can define different locations.  You can treat a location either as a global setting for your entire organization or physical location broken up by DHCP Servers.

<a href="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/11/3-2.png" rel="attachment wp-att-1337"><img class="alignnone wp-image-1337" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/11/3-2-1024x323.png?resize=600%2C189&#038;ssl=1" alt="3" width="600" height="189" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/11/3-2.png?resize=1024%2C323&ssl=1 1024w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/11/3-2.png?resize=300%2C95&ssl=1 300w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/11/3-2.png?resize=768%2C243&ssl=1 768w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/11/3-2.png?w=1064&ssl=1 1064w" sizes="(max-width: 600px) 100vw, 600px" data-recalc-dims="1" /></a>

In our example, we have two different locations.  The first is our Main Office and the second is the MSP Office.  You will notice we always remove spaces on names (you will see why in a second).  Additionally we see if provisioning files have been generated for that location and the option to delete that location.

When we open up the settings for the location a few things to note.

<a href="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/11/4-3.png" rel="attachment wp-att-1339"><img class="alignnone wp-image-1339" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/11/4-3-1024x552.png?resize=600%2C323&#038;ssl=1" alt="4" width="600" height="323" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/11/4-3.png?resize=1024%2C552&ssl=1 1024w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/11/4-3.png?resize=300%2C162&ssl=1 300w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/11/4-3.png?resize=768%2C414&ssl=1 768w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/11/4-3.png?w=1074&ssl=1 1074w" sizes="(max-width: 600px) 100vw, 600px" data-recalc-dims="1" /></a>

First, we have moved from HTTPS to an FTPS site.  This allows individuals devices to upload log files, call logs and much more.  Each location has it&#8217;s own provisioning server URL.  A single username and password is used for all locations in your deployment.

In addition to that change, you will notice that all settings are displayed in an easy to use tab interface making organized by section.  You can also download your master and custom config files if you want to use your own on-prem provisioning server.

<span style="text-decoration: underline">Groups</span>

Once you have created your location configuration you can now create as many groups as you want for your organization.

<a href="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/11/5-1.png" rel="attachment wp-att-1340"><img class="alignnone wp-image-1340" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/11/5-1-1024x249.png?resize=600%2C146&#038;ssl=1" alt="5" width="600" height="146" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/11/5-1.png?resize=1024%2C249&ssl=1 1024w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/11/5-1.png?resize=300%2C73&ssl=1 300w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/11/5-1.png?resize=768%2C187&ssl=1 768w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/11/5-1.png?w=1070&ssl=1 1070w" sizes="(max-width: 600px) 100vw, 600px" data-recalc-dims="1" /></a>

Devices can ultimately be assigned to the groups created.  Each group will have it&#8217;s own configuration which looks identical to the above location configuration.  The group configuration will be initially built upon the location configuration but you can customize the group however you want.

<span style="text-decoration: underline">Devices</span>

When you go to devices you will see all devices for that location.  Any device that has uploaded it&#8217;s log files to the FTPS server will automatically be displayed here.  Additionally, you can add your own list of devices.

<a href="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/11/6-1.png" rel="attachment wp-att-1342"><img class="alignnone wp-image-1342" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/11/6-1.png?resize=600%2C160&#038;ssl=1" alt="6" width="600" height="160" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/11/6-1.png?w=996&ssl=1 996w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/11/6-1.png?resize=300%2C80&ssl=1 300w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/11/6-1.png?resize=768%2C205&ssl=1 768w" sizes="(max-width: 600px) 100vw, 600px" data-recalc-dims="1" /></a>

When you view the details of a device, you will see the model, firmware installed and when it last was updated/checked in.  From here you can assign a device to a group, view files including log/config, etc.

<a href="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/11/7-1.png" rel="attachment wp-att-1341"><img class="alignnone wp-image-1341" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2016/11/7-1-1024x286.png?resize=600%2C168&#038;ssl=1" alt="7" width="600" height="168" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/11/7-1.png?resize=1024%2C286&ssl=1 1024w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/11/7-1.png?resize=300%2C84&ssl=1 300w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/11/7-1.png?resize=768%2C214&ssl=1 768w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2016/11/7-1.png?w=1060&ssl=1 1060w" sizes="(max-width: 600px) 100vw, 600px" data-recalc-dims="1" /></a>

What else is coming.

  * Being able to assign devices to group in a mass process
  * Better UI updates so its easier to figure out
  * Fix a chicken/egg scenario with devices and groups. Right now when you assign a device to a group, you have to go back and re-generate files for the entire group.

Remember, these are features that are coming by the end of the year but wanted to give a sneak peak of what we are working on.  If you would like to get involved with testing the phone provisioning features feel free to drop me an e-mail at richard -at- masteringlync.com and I can get you access to the beta.  We are looking for only about 5 people to start with.