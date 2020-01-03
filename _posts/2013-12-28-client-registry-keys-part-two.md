---
id: 495
title: Client Registry Keys (Part Two)
date: 2013-12-28T23:53:20+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=495
permalink: /2013/12/28/client-registry-keys-part-two/
st_layout_box:
  - right
panels_data:
  - 'a:0:{}'
categories:
  - Lync Server 2013
---
[Client Registry Keys – Part One](http://masteringlync.com/2013/09/19/client-registry-keys/) (General & Personal Tab)

[Client Registry Keys – Part Two](http://masteringlync.com/2013/12/28/client-registry-keys-part-two/) (Contacts List & Status Tab)

[Client Registry Keys – Part Three](http://masteringlync.com/2014/02/26/client-registry-keys-part-3/) (My Picture & Phones Tab)

[Client Registry Keys – Part Four](http://masteringlync.com/2014/04/14/client-registry-keys-part-4/) (Alerts, Persistent Chat & Ringtones and Sounds)

[Client Registry Keys – Part Five](http://masteringlync.com/2014/08/13/client-registry) (Audio Device, Video Device, Call Forwarding, File Saving, Recording & Lync Meetings)

[Client Registry Keys &#8211; Part Six](http://masteringlync.com/2014/10/14/client-registry-keys-part-six/) (IM Tab)

&nbsp;

Here is the second post regarding client registry keys.  I will again give you the registry key and client policy (if one exists).  I will continue to update as new features get added.

**Contact Lists Tab**

<img class="alignnone wp-image-497 size-full" src="https://masteringlync.com/wp-content/2013/11/pic11.png?resize=300%2C245&ssl=1 300w" sizes="(max-width: 730px) 100vw, 730px" data-recalc-dims="1" />

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
      Display my contacts (two lines)
    </td>
    
    <td>
      TwoLineView
    </td>
    
    <td>
      1
    </td>
    
    <td>
      N/A
    </td>
  </tr>
  
  <tr>
    <td>
      Display my contacts (one line)
    </td>
    
    <td>
      TwoLineView
    </td>
    
    <td>
    </td>
    
    <td>
      N/A
    </td>
  </tr>
  
  <tr>
    <td>
      Order by list (By name)
    </td>
    
    <td>
      SortContactsByName
    </td>
    
    <td>
      1
    </td>
    
    <td>
      N/A
    </td>
  </tr>
  
  <tr>
    <td>
      Order by list (By availability)
    </td>
    
    <td>
      SortContactsByName
    </td>
    
    <td>
    </td>
    
    <td>
      N/A
    </td>
  </tr>
  
  <tr>
    <td>
      Contact name (instead of e-mail address)
    </td>
    
    <td>
      ShowContactFriendlyName
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
      Contact status
    </td>
    
    <td>
      ShowContactStatus
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
      Show contacts with away, offline in a separate group
    </td>
    
    <td>
      ShowOnLineContactsOnly
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
      Favorites Group
    </td>
    
    <td>
      ShowFavoriteContacts
    </td>
    
    <td>
      0/1
    </td>
    
    <td>
      N/A
    </td>
  </tr>
</table>

**Status Tab**

<img class="alignnone wp-image-498 size-full" src="https://masteringlync.com/wp-content/uploads/2013/11/pic21.png?resize=300%2C245&ssl=1 300w" sizes="(max-width: 731px) 100vw, 731px" data-recalc-dims="1" />

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
      Show me as inactive when &#8230;
    </td>
    
    <td>
      IdleThreshold
    </td>
    
    <td>
      (in minutes)
    </td>
    
    <td>
      N/A
    </td>
  </tr>
  
  <tr>
    <td>
      Change my status from Inactive to away &#8230;
    </td>
    
    <td>
      AwayThreshold
    </td>
    
    <td>
      (in minutes)
    </td>
    
    <td>
      N/A
    </td>
  </tr>
  
  <tr>
    <td>
      I want everyone to be able to see my presence &#8230;
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
      I want the system administrator to decide &#8230;
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
      Show me as Do Not Disturb when I present my desktop
    </td>
    
    <td>
      EnableLyncSharingPresenting
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
      Show me as Do Not Disturb when my monitor is duplicated
    </td>
    
    <td>
      DuplicatePrimaryMonitorPresentingSetting
    </td>
    
    <td>
      1/2 *
    </td>
    
    <td>
      N/A
    </td>
  </tr>
</table>

* 1 = off, 2 = on