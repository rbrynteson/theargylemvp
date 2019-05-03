---
id: 383
title: Client Registry Keys (Part One)
date: 2013-09-19T04:00:05+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=383
permalink: /2013/09/19/client-registry-keys/
st_layout_box:
  - right
panels_data:
  - 'a:0:{}'
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
categories:
  - Featured
  - Lync Server 2013
---
[Client Registry Keys – Part One](http://masteringlync.com/2013/09/19/client-registry-keys/) (General & Personal Tab)

[Client Registry Keys – Part Two](http://masteringlync.com/2013/12/28/client-registry-keys-part-two/) (Contacts List & Status Tab)

[Client Registry Keys – Part Three](http://masteringlync.com/2014/02/26/client-registry-keys-part-3/) (My Picture & Phones Tab)

[Client Registry Keys – Part Four](http://masteringlync.com/2014/04/14/client-registry-keys-part-4/) (Alerts, Persistent Chat & Ringtones and Sounds)

[Client Registry Keys – Part Five](http://masteringlync.com/2014/08/13/client-registry) (Audio Device, Video Device, Call Forwarding, File Saving, Recording & Lync Meetings)

[Client Registry Keys &#8211; Part Six](http://masteringlync.com/2014/10/14/client-registry-keys-part-six/) (IM Tab)

&nbsp;

[UPDATED 10/14/2014 &#8211; October CU added IM tab.  See Part Six for additional information]

[UPDATED 7/7/2014 &#8211; Additional information about saving IM history and automatic saving of IM&#8217;s]

[UPDATED 9/21/2013 &#8211; With new client features that include spell check]

&nbsp;

So I&#8217;ve been working on a client where we needed to push out some settings via registry key as there was no corresponding in-band provisioning.  So I decided to take to the Bing&#8217;s and see if I could find them all.  I found Tom&#8217;s blog:

http://lyncdup.com/2012/11/what-lync-2013-client-setting-are-held-in-the-registry/

Which gave me the list of possible values.  What I wasn&#8217;t sure about was what the values would need to be and then found a few keys that Tom didn&#8217;t have in his blog post.  So I decided, why not go through the client page by page and see if I could find each registry key, what values would be supported and of course find the corresponding set-csclientpolicy value.  Why would I do this you might ask?  Because I&#8217;m a nerd and find this pretty neat.  So this will take a few posts to get through it all.

A few basics to be aware of.

All of the registry keys will be found in the HKCU:SoftwareMicrosoftOffice15.0Lync

Almost all of the values are DWORD values.  Unless otherwise noted that is what they are.

The values are 0 = unchecked/disabled and 1 = checked/enabled.  Unless otherwise noted.

[<img class="alignnone wp-image-856 size-full" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/09/pic22.png?resize=712%2C580&#038;ssl=1" alt="pic2" width="712" height="580" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/09/pic22.png?w=712&ssl=1 712w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/09/pic22.png?resize=300%2C244&ssl=1 300w" sizes="(max-width: 712px) 100vw, 712px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.com/files/2013/09/pic22.png)

<table width="100%">
  <tr>
    <td>
      Description
    </td>
    
    <td>
      Reg Key
    </td>
    
    <td style="width: 100px">
      Values
    </td>
    
    <td>
      Client Policy
    </td>
  </tr>
  
  <tr>
    <td>
       Reopen my conversations when I sign in to Lync
    </td>
    
    <td>
       IsConversationStatePreservationEnabled
    </td>
    
    <td>
       0/1
    </td>
    
    <td>
       N/A
    </td>
  </tr>
  
  <tr>
    <td>
       Sign up for customer experience improvement &#8230;
    </td>
    
    <td>
    </td>
    
    <td>
    </td>
    
    <td>
       EnableSQMData (ONLY FOR LYNC 2010)
    </td>
  </tr>
  
  <tr>
    <td>
       Automatically send lync error info to Microsoft
    </td>
    
    <td>
    </td>
    
    <td>
    </td>
    
    <td>
    </td>
  </tr>
  
  <tr>
    <td>
       Logging in Lync
    </td>
    
    <td>
       EnableTracing
    </td>
    
    <td>
       0/1
    </td>
    
    <td>
       EnableTracing
    </td>
  </tr>
  
  <tr>
    <td>
       Also turn on Windows Event Logging
    </td>
    
    <td>
       EnableEventLogging
    </td>
    
    <td>
       0/1
    </td>
    
    <td>
       EnableEventLogging
    </td>
  </tr>
  
  <tr>
    <td>
       Minimize to the notification area instead of task bar
    </td>
    
    <td>
       MinimizeWindowToNotificationArea
    </td>
    
    <td>
       0/1
    </td>
    
    <td>
       N/A
    </td>
  </tr>
</table>

&nbsp;

**Personal Tab**

[<img class="alignnone wp-image-857 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/09/pic31.png?resize=712%2C581&#038;ssl=1" alt="pic3" width="712" height="581" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/09/pic31.png?w=712&ssl=1 712w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/09/pic31.png?resize=300%2C245&ssl=1 300w" sizes="(max-width: 712px) 100vw, 712px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.com/files/2013/09/pic31.png)

<table width="100%">
  <tr>
    <td>
      Description
    </td>
    
    <td>
      Reg Key
    </td>
    
    <td style="width: 100px">
      Values
    </td>
    
    <td>
      Client Policy
    </td>
  </tr>
  
  <tr>
    <td>
       Sign-in address
    </td>
    
    <td>
       ServerSipUri
    </td>
    
    <td>
       REG_SZ
    </td>
    
    <td>
       N/A
    </td>
  </tr>
  
  <tr>
    <td>
       Automatically start Lync when I log on to Windows
    </td>
    
    <td>
       SEE BELOW #1
    </td>
    
    <td>
       &#8212;
    </td>
    
    <td>
       N/A
    </td>
  </tr>
  
  <tr>
    <td>
       Show Lync in the foreground when it starts
    </td>
    
    <td>
       AutoOpenMainWindowWhenStartup
    </td>
    
    <td>
       0/1
    </td>
    
    <td>
       N/A
    </td>
  </tr>
  
  <tr>
    <td>
       Personal Information Manager
    </td>
    
    <td>
    </td>
    
    <td>
    </td>
    
    <td>
       N/A
    </td>
  </tr>
  
  <tr>
    <td>
       Update my presence based on my &#8230;
    </td>
    
    <td>
    </td>
    
    <td>
    </td>
    
    <td>
       DisableCalendarPresence
    </td>
  </tr>
  
  <tr>
    <td>
       Show meeting subject and location &#8230;
    </td>
    
    <td>
    </td>
    
    <td>
    </td>
    
    <td>
       DisableMeetingSubjectAndLocation
    </td>
  </tr>
  
  <tr>
    <td>
       Show my Out of Office info &#8230;
    </td>
    
    <td>
    </td>
    
    <td>
    </td>
    
    <td>
       DisablePresenceNote
    </td>
  </tr>
  
  <tr>
    <td>
       Save IM conversations in my email Conversation &#8230;
    </td>
    
    <td>
       SEE BELOW #2
    </td>
    
    <td>
       &#8212;
    </td>
    
    <td>
       EnableIMAutoArchiving
    </td>
  </tr>
  
  <tr>
    <td>
       Save call logs in my email conversation history folder
    </td>
    
    <td>
       SEE BELOW #2
    </td>
    
    <td>
       &#8212;
    </td>
    
    <td>
       EnableCallLogAutoArchiving
    </td>
  </tr>
  
  <tr>
    <td>
       Share my location info with other programs I use
    </td>
    
    <td>
       N/A
    </td>
    
    <td>
       &#8212;
    </td>
    
    <td>
       N/A
    </td>
  </tr>
  
  <tr>
    <td>
       Show pictures of contacts
    </td>
    
    <td>
       ShowPhoto
    </td>
    
    <td>
       0/1
    </td>
    
    <td>
       DisplayPhoto
    </td>
  </tr>
</table>

&nbsp;

#1 Automatically start Lync Client

<table border="0" width="100%" cellspacing="0" cellpadding="0">
  <tr>
    <td valign="top" width="174">
      Registry Location
    </td>
    
    <td valign="top" width="450">
      HKCUSoftwareMicrosoftWindowsCurrentVersionRun
    </td>
  </tr>
  
  <tr>
    <td valign="top" width="174">
      Registry Key Name
    </td>
    
    <td valign="top" width="450">
      Lync
    </td>
  </tr>
  
  <tr>
    <td valign="top" width="174">
      Registry Key Type
    </td>
    
    <td valign="top" width="450">
      REG_SZ
    </td>
  </tr>
  
  <tr>
    <td valign="top" width="174">
      Registry Key Value
    </td>
    
    <td valign="top" width="450">
      &#8220;C:Program FilesMicrosoft Office 15rootoffice15lync.exe&#8221; /fromrunkey
    </td>
  </tr>
</table>

&nbsp;

#2 &#8211; Saving Conversation History

This one gets asked all the time and I&#8217;ve confirmed, this data cannot be set via the registry.  This information, if not set by your administrator, is actually stored in the back-end database.  As a test, try the following: Go to client #1 and uncheck Save IM conversations &#8230; go to client #2, log out of Lync and log back in (with same user of course) and you will see it&#8217;s unchecked.  <del>This can only happen because when you clear the setting it goes into the database.  I&#8217;ll scrub the database and see if I can find it.</del>

One item to remember is that EnableIMAutoArchiving just stops the automatic saving of IM&#8217;s to conversation history.  A user could still use the CTRL+S to save it themselves.  You would need to also enable the _DisableSavingIM_ client policy tag to completely stop saving of all IM&#8217;s.

You can learn all about this field here: <http://masteringlync.com/2013/11/11/the-imautoarchiving-flag/>

I&#8217;m in the process of finishing up this table.  I&#8217;ve found some old references for OCS 2007/R2/2010 but they don&#8217;t apply to the 2013 client.  So I&#8217;m going to continue to fish through the registry for them.  Once I&#8217;ve finished those I&#8217;ll move onto the next screen.