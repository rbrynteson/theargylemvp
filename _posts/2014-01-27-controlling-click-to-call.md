---
id: 607
title: Controlling Click-to-Call
date: 2014-01-27T19:42:53+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=607
permalink: /2014/01/27/controlling-click-to-call/
st_layout_box:
  - right
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
---
The Lync 2013 client simplifies the work place and one of the cornerstone features is the ability to use click-to-call.  For an Enterprise Voice user like myself, I cannot remember the last time I picked up a handset and dialed a user.  And I certainly never use the &#8220;dial-pad&#8221; that is provided in the client.  Rather, I use click-to-call for all conversations. One of my co-workers recently asked me a question related to the click-to-call.  &#8220;Can you force the client to always use Lync call by default?&#8221;  They never want to see this:<figure id="attachment_608" style="width: 282px" class="wp-caption alignnone">

[<img class="wp-image-608 size-full" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/01/pic1.png?resize=282%2C71&#038;ssl=1" alt="pic1" width="282" height="71" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.com/files/2014/01/pic1.png)<figcaption class="wp-caption-text">Client has default to Mobile call because that was the last one used.</figcaption></figure> 

The question seemed straight forward enough so I started to do a little digging around the Lync 2013 client for the details.  (NOTE: Although this post is about the Lync 2013 client, the settings and behavior appears to be exactly the same in Lync 2010, the registry information is simply in a different place.) **Client Policy** There are plenty of people (<a href="http://trogjels.wordpress.com/2011/04/12/lync-default-call/" target="_blank">here</a>, <a href="http://howexchangeworks.com/2013/09/make-lync-call-the-default-in-lync-2010-client.html" target="_blank">here</a>) who have discovered the <a href="http://technet.microsoft.com/en-us/library/gg398300.aspx" target="_blank"><em>EnableVOIPCallDefault</em> </a>option as part of the client policy.  But what is important to note is that, this is simply the default option for the client for click-to-call.  If a user selects another number (work, mobile, etc.) this information is than saved and changed to the default.  So while this is an important first step, this doesn&#8217;t solve the entire problem. Therefore you should execute: _get-csclientpolicy | set-csclientpolicy -EnableVOIPCallDefault $true_ **More to the Story** Although that client policy is a nice setting, the moment a user does a click-to-call and selects anything but Lync Call, the client is now going to remember that information going forward.  If you simply click the call button (without using the drop down &#8211; which is frankly too hidden the Lync 2013 client) than the call will go to the wrong location. Where does this information get saved? Thankfully, this is stored in the registry on each workstation and simply deleting this information resets back to the default.<figure id="attachment_609" style="width: 645px" class="wp-caption alignnone">

[<img class="wp-image-609 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/01/pic2.png?resize=645%2C101&#038;ssl=1" alt="pic2" width="645" height="101" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/01/pic2.png?w=645&ssl=1 645w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/01/pic2.png?resize=300%2C47&ssl=1 300w" sizes="(max-width: 645px) 100vw, 645px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.com/files/2014/01/pic2.png)<figcaption class="wp-caption-text">HKCU/Software/Microsoft/Office15.0/Lync/user@domain.com/ContactStatusCacheU/sip:usercalled@domain.com</figcaption></figure> 

**Options** Thus far I&#8217;ve found two options. _Option #1 &#8211; Restrict Access to the Key_ In my first scenario I simply restricted my user account from being able to read/write to the ContactStatusChachU key.  The client displays no errors when I make phone calls and select something other than Lync Call, and even in my scenario where I already had information stored in the key it still defaulted back to Lync call.

[<img class="alignnone wp-image-610 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/01/pic3.png?resize=331%2C194&#038;ssl=1" alt="pic3" width="331" height="194" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/01/pic3.png?w=331&ssl=1 331w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/01/pic3.png?resize=300%2C176&ssl=1 300w" sizes="(max-width: 331px) 100vw, 331px" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.com/files/2014/01/pic3.png)

_Option #2 &#8211; Delete the Key_ The second option I&#8217;ve thought of was simply deleting the key on log out.  The trick here is that you will need to write a script that will only delete the contents of the ContactStateCacheU directory.  Since this key is stored within a key that has the users SIP domain as a parent key, we will need to loop through the registry keys and delete only that key. I&#8217;ll update this post once I spent a little time to determine the best way to script this out.  I could simply write a quick C# application that runs on log-off but maybe that is a bit much? NOTE: I guess this goes without saying but you should test this out in your environment and editing the registry is one of those &#8220;use at your own risk&#8221; scenarios.