---
id: 1215
title: 'Skype for Business CU1 &#8211; Shared Line Appearance'
date: 2015-11-18T02:19:10+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=1215
permalink: /2015/11/18/skype-for-business-cu1-shared-line-appearance/
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
  - Skype for Business
---
Skype for Business Cumulative Update 1 (CU1) is finally here.  No, all of those other patches were not CU 1.  They were CU 0.1 and 0.2 &#8211; but now we have the real deal.

You can download the update here: https://www.microsoft.com/en-us/download/details.aspx?id=47690

&#8220;Details&#8221; of SLA can be found here: https://support.microsoft.com/en-us/kb/3092727

**Setup and Configuration**

I say &#8220;details&#8221; because the docs are not very detailed.  Here is what you need to know.

  1. Of course, install CU1 on your servers.   Make sure to read them carefully.  Don&#8217;t skip any steps!
  2. Register the SLA endpoint.  I haven&#8217;t seen any docs for this yet but the command is here:New-CsServerApplication -Identity &#8216;Service:Registrar:%FQDN%/SharedLineAppearance&#8217; -Uri <http://www.microsoft.com/LCS/SharedLineAppearance> -Critical $false -Enabled $true -Priority (Get-CsServerApplication -Identity &#8216;Service:Registrar:%FQDN%/UserServices&#8217;).PriorityWhere %FQDN% is your server name or pool.  This will create your server application and should look a lot like this when done.[<img class="alignnone size-full wp-image-1216" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/11/12.png?resize=653%2C112&#038;ssl=1" alt="1" width="653" height="112" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/11/12.png?w=653&ssl=1 653w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/11/12.png?resize=300%2C51&ssl=1 300w" sizes="(max-width: 653px) 100vw, 653px" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/11/12.png)
  3. Run &#8220;Update-CsAdminRole&#8221; from the Skype for Business 2015 Management Shell
  4. Stop/Start the front-end service (or services if you have an Enterprise Pool).

Your server configuration is now complete.  Lets move to the user/object config now.

  1. Create an Enterprise Voice user who is going to be your Shared Line Appearance.  In my mind, this isn&#8217;t a person &#8211; as that scenario is covered under the existing Boss/Admin.  Instead, I think of this as main line, shared human resources line, etc.  Basically, something you might have used RGS for in the past.  Make sure to assign the user a LineURI and home to your SfB 2015 CU1 Pool.
  2. Create the SLA Object.  **Set-CsSlaConfiguration -Identity <IdentityOfGroup> -MaxNumberOfCalls <Number> -BusyOption <BusyOnBusy|Voicemail|Forward> [-Target <TargetUserOrPhoneNumber>] -MissedCallOption <Disconnect,Forward,BusySignal> [-MissedCallForwardTarget <TargetUserorPhoneNumber>]**You can see you have various options here for the Busy Option.  Essentially, what should we do if we have hit the max number of calls.  Likewise, we have a missed call forward as well.  So in my case, I&#8217;m going to create an SLA with three lines that gives a Busy on Busy result.[<img class="alignnone size-full wp-image-1217" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/11/21.png?resize=649%2C138&#038;ssl=1" alt="2" width="649" height="138" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/11/21.png?w=649&ssl=1 649w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/11/21.png?resize=300%2C64&ssl=1 300w" sizes="(max-width: 649px) 100vw, 649px" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/11/21.png)
  3. Next we are going to add myself as a delegate to this SLA object.Add-CsSlaDelegates -Identity <IdentityOfGroup> -Delegate <NameOfDelegate@domain>[<img class="alignnone size-full wp-image-1218" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/11/31.png?resize=650%2C143&#038;ssl=1" alt="3" width="650" height="143" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/11/31.png?w=650&ssl=1 650w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/11/31.png?resize=300%2C66&ssl=1 300w" sizes="(max-width: 650px) 100vw, 650px" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/11/31.png)

At this time I am now a delegate of the Main Line SLA object.  On my Polycom VVX phone I can see three lines for Main Line.  (Sorry for the crappy screen shots.  My VVX won&#8217;t let me login to take Screen Captures &#8211; most likely because I&#8217;m doing it wrong.)

VVX500

[<img class="alignnone size-full wp-image-1219" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/11/4.png?resize=511%2C372&#038;ssl=1" alt="4" width="511" height="372" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/11/4.png?w=511&ssl=1 511w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/11/4.png?resize=300%2C218&ssl=1 300w" sizes="(max-width: 511px) 100vw, 511px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/11/4.png)

VVX400

[<img class="alignnone size-full wp-image-1220" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/11/5.png?resize=451%2C344&#038;ssl=1" alt="5" width="451" height="344" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/11/5.png?w=451&ssl=1 451w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/11/5.png?resize=300%2C229&ssl=1 300w" sizes="(max-width: 451px) 100vw, 451px" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/11/5.png)

NOTE: Your VVX phone needs to be running 5.4.0A (10182) for this to even work.  Official support comes in 5.4.1 which is upcoming.

**Usage**

The actual usage of SLA isn&#8217;t super exciting to be honest.  The call rings the user just like a normal delegate call would.  You will see the toaster pop and it will way &#8220;Call For &#8230; &#8221; like normal.  Where the interesting part comes is when you place the call on hold.

On the phone, you will find that your call is now blinking on Line 1.  Now someone else who is a delegate for this SLA group will have the ability to pick the call that was on hold.  Additionally, you will see how many people are on a call at a given time as well.

[<img class="alignnone size-full wp-image-1221" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/11/6.png?resize=612%2C387&#038;ssl=1" alt="6" width="612" height="387" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/11/6.png?w=612&ssl=1 612w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/11/6.png?resize=300%2C190&ssl=1 300w" sizes="(max-width: 612px) 100vw, 612px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/11/6.png)

**How Does It Work?**

Under the hood we have an object registered to the front-end server to keep track of the calls (the Server Application) and a user who has delegation enabled.  Here is what it looks like now that I login to my user and look at call forwarding.

[<img class="alignnone size-full wp-image-1222" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/11/7.png?resize=500%2C356&#038;ssl=1" alt="7" width="500" height="356" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/11/7.png?w=500&ssl=1 500w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/11/7.png?resize=300%2C214&ssl=1 300w" sizes="(max-width: 500px) 100vw, 500px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/11/7.png)

**What Doesn&#8217;t It Do?**

The biggest issue I have with this implication of SLA is with VVX 300/400 phones (and the 500/600 but to a lesser extent).  When you look at the phone (see the picture above) we have these three lines and there is a physical button next to each one.  This of course, gives you the appearance that you can click one of these buttons and make an outbound call as that number.  Unfortunately, this isn&#8217;t the case.  Nothing happens when you click the button.  It just sits there.  Pick up the phone and the call goes out as you.

Maybe Polycom can work some of their magic and make it possible.

**Other Commands**

There are a bunch of other CU1 commands added and they are here.

  * Add-CsSlaDelegates
  * Remove-CsSlaDelegates
  * Get-CsSlaConfiguration
  * Set-CsSlaConfiguration
  * Remove-CsSlaConfigurgetation
  * New-CsGroupPickupUserOrbit
  * Get-CsGroupPickupUserOrbit
  * Set-CsGroupPickupUserOrbit
  * Remove-CsGroupPickupUserOrbit
  * Reset-CsNotificationQueues
  * Get-CsServerPatchVersion

A lot of them require no explanation.  Hope that helps.

&nbsp;