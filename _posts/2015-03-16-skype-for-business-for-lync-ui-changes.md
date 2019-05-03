---
id: 1091
title: Skype for Business vs Lync UI Changes
date: 2015-03-16T13:00:09+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=1091
permalink: /2015/03/16/skype-for-business-for-lync-ui-changes/
panels_data:
  - 'a:0:{}'
categories:
  - Uncategorized
---
**Overview**

One of the most noticeable changes from Skype for Business (SfB) to Lync Server 2013 is the client user interface.

First, the Lync 2013 client will simply become the SfB client.  The client will ship with two UI&#8217;s or skins.  This will allow administrators to choose which one works best for their environment.

Second, the SfB client is going to be distributed via Windows Update as an Office update.  The UI is going to be controlled initially based on the backend server platform you have.  That is the client will detect if you are homed on Lync 2013 and present the 2013 UI.  If you connect to a SfB backend, it will display that UI.  However, that said, you have the ability to control the UI via policy as well.  This would allow you to upgrade the backend of your deployment, leave the Lync 2013 UI for the time being until you are ready to train, etc.

**Control the UI**

The ability to control the UI is done via a client policy.

[<img class="alignnone wp-image-1092 size-full" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/03/1.png?resize=604%2C67&#038;ssl=1" alt="1" width="604" height="67" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/03/1.png?w=604&ssl=1 604w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/03/1.png?resize=300%2C33&ssl=1 300w" sizes="(max-width: 604px) 100vw, 604px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/03/1.png)

You can update your client policy using the following command:

> Get-CsClientPolicy | Set-CsClientPolicy -EnableSkypeUI $true

During the testing of the new client it has been rumored that an update to the Lync Server 2013 will introduce the new EnableSkypeUI for those organizations not ready to move to SfB Server 2015.  I don&#8217;t know when this patch is going to be available or if one will be coming to Lync Server 2010.

However, you can control the client via registry as well.  The client policy is creating a local registry in the following location: HKEY\_CURRENT\_USER\Software\Microsoft\Office\Lync\EnableSkypeUI

[<img class="alignnone wp-image-1094 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/03/2.png?resize=651%2C225&#038;ssl=1" alt="2" width="651" height="225" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/03/2.png?w=651&ssl=1 651w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/03/2.png?resize=300%2C104&ssl=1 300w" sizes="(max-width: 651px) 100vw, 651px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/03/2.png)

The EnableSkypeUI REG\_BINARY can be set to either 01 00 00 00 (True) or 00 00 00 00 (False).  What I don&#8217;t understand is why a REG\_DWORD wasn&#8217;t used instead.  Pushing REG_BINARY can be a bit of a pain so hopefully the Lync Server 2013 updates will be released soon.

**UI Comparison**

Here is a side-by-side comparison of the two clients.  Note that I&#8217;m using the EnableSkypeUI flag to flip between the UI&#8217;s so the build number of the clients are exactly the same.

[<img class="alignnone size-column2-1/2 wp-image-1096" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/03/Compare-570x285.png?resize=570%2C285&#038;ssl=1" alt="Compare" width="570" height="285" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/03/Compare.png)

There are some things that are immediately noticeable.  First, we have moved to the over used picture in a circle look.  Second, the default view takes up much more space then the old client.  Using the same resolution and window size we can see that we can&#8217;t fit as many people into the visible window.  That is unfortunate in my mind. There is plenty of white space between the contacts that could be removed.  Third, the presence icons have changed to small round circles instead of the color bar next to the pictures.  I&#8217;ll be honest this takes a bit of time to get used to.  I&#8217;ve looked at the client more than once and didn&#8217;t even notice the presence of the user.

Another change you can&#8217;t see in these pictures is that the default sounds have all changed from the Lync to Skype sounds.  I would highly recommend that you take a backup of the Lync sounds and distribute them to your users.   I understand the desire to make the new UI look and feel like the consumer version but changing the sound files may cause some administrative headaches.  Even running the client in Lync 2013 UI mode the sounds are still the new SfB sounds which might be unnerving for some users and result in unnecessary helpdesk calls.

I think the changes overall are a positive.  I have some issues with the UI much like I did when we moved from Lync 2010 to Lync 2013 but I do believe the client will resonate with end users.

**Client Options**

[<img class="alignnone wp-image-1099 size-full" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/03/3.png?resize=778%2C121&#038;ssl=1" alt="3" width="778" height="121" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/03/3.png?w=778&ssl=1 778w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/03/3.png?resize=300%2C47&ssl=1 300w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/03/3.png?resize=768%2C119&ssl=1 768w" sizes="(max-width: 778px) 100vw, 778px" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/03/3.png)

The SfB Client comes with a floating call control window that appears whenever the client is in the background.  This feature can be enabled or disabled in the General Settings.  This is also controlled in the registry via a DWORD entry of CallMonitorEnabled.

[<img class="alignnone wp-image-1101" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/03/4.png?resize=500%2C136&#038;ssl=1" alt="4" width="500" height="136" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/03/4.png?w=857&ssl=1 857w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/03/4.png?resize=300%2C82&ssl=1 300w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/03/4.png?resize=768%2C209&ssl=1 768w" sizes="(max-width: 500px) 100vw, 500px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/03/4.png)

You can now control where the toaster pop appears in the SfB Client.  This is a significant change and one people have been asking requesting for a while.  While there are third party applications that do the same (and some do even more) it&#8217;s a welcome add to the client.

[<img class="alignnone size-full wp-image-1102" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/03/5.png?resize=572%2C277&#038;ssl=1" alt="5" width="572" height="277" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/03/5.png?w=572&ssl=1 572w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/03/5.png?resize=300%2C145&ssl=1 300w" sizes="(max-width: 572px) 100vw, 572px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/03/5.png)

One of the huge complaints of Lync 2013 was the emoticons.  I never quite understood the rage but Microsoft has introduced the animated emoticons from the Skype Consumer client to the SfB Client.  However some people may not want to see a smily face vomit so you have the option to disable the animations.  This is controlled in the registry with EnableEmoticonAnimation.

There is a new Call Handling menu if you enable Call via Work but that will be discussed in-depth later on.

&nbsp;