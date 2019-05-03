---
id: 799
title: Client Registry Keys (Part 5)
date: 2014-08-13T02:38:16+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=799
permalink: /2014/08/13/client-registry-keys-part-5/
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

**Audio Device Tab**

[<img class="alignnone wp-image-800 size-medium" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/08/pic1-300x252.png?resize=300%2C252&#038;ssl=1" alt="pic1" width="300" height="252" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/08/pic1.png?resize=300%2C252&ssl=1 300w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/08/pic1.png?w=732&ssl=1 732w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.com/files/2014/08/pic1.png)

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
      Secondary Ringer
    </td>
    
    <td>
      SecondaryRingerIsEnabled
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
      Also Ring
    </td>
    
    <td>
      N/A
    </td>
    
    <td>
      N/A
    </td>
    
    <td>
      N/A
    </td>
  </tr>
  
  <tr>
    <td>
      Unmute when my phone rings
    </td>
    
    <td>
      UnmuteSpeakerOnIncomingCall
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
      Allow stereo audio playback when available
    </td>
    
    <td>
      N/A
    </td>
    
    <td>
      N/A
    </td>
    
    <td>
      N/A
    </td>
  </tr>
</table>

These settings are in a different place than all of the other client registry settings.  You can find them all here:

HKEY\_CURRENT\_USERSoftwareMicrosoftUCCPlatformLync

**Video Device Tab**

[<img class="alignnone wp-image-801 size-medium" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/08/pic2-300x240.png?resize=300%2C240&#038;ssl=1" alt="pic2" width="300" height="240" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/08/pic2.png?resize=300%2C240&ssl=1 300w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/08/pic2.png?w=716&ssl=1 716w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.com/files/2014/08/pic2.png)

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
      Crop and center my video in meetings
    </td>
    
    <td>
      CropOutgoingVideo
    </td>
    
    <td>
      0/1
    </td>
    
    <td>
      N/A
    </td>
  </tr>
</table>

These settings are in the normal place.  No need to hunt around.  (HKEY\_CURRENT\_USERSoftwareMicrosoftOffice15.0Lync)

**Call Forwarding Tab**

All of these settings are stored in your presence documents.  So we will leave that for another time.

**File Saving Tab**

[<img class="alignnone wp-image-802 size-medium" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/08/pic3-300x240.png?resize=300%2C240&#038;ssl=1" alt="pic3" width="300" height="240" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/08/pic3.png?resize=300%2C240&ssl=1 300w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/08/pic3.png?w=721&ssl=1 721w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.com/files/2014/08/pic3.png)

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
      Save to
    </td>
    
    <td>
      FtReceiveFolder
    </td>
    
    <td>
      C:UsersRichardDocumentsMy Received Files
    </td>
    
    <td>
      N/A
    </td>
  </tr>
</table>

**Recording Tab**

[<img class="alignnone wp-image-803 size-medium" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/08/pic4-300x242.png?resize=300%2C242&#038;ssl=1" alt="pic4" width="300" height="242" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/08/pic4.png?resize=300%2C242&ssl=1 300w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/08/pic4.png?w=717&ssl=1 717w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.com/files/2014/08/pic4.png)

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
      Save to
    </td>
    
    <td>
      RecordingRootDirectory1
    </td>
    
    <td>
      C:UsersRichardVideosLync
    </td>
    
    <td>
      N/A
    </td>
  </tr>
  
  <tr>
    <td>
      Image Resolution
    </td>
    
    <td>
      PublishResolution
    </td>
    
    <td>
      0 &#8211; 480p<br /> 1 &#8211; 720p HD<br /> 2 &#8211; 1080p Full HD (Highest)
    </td>
    
    <td>
      N/A
    </td>
  </tr>
</table>

These settings are found here:

HKEY\_CURRENT\_USERSoftwareMicrosoftOffice15.0LyncRecording

**Lync Meetings Tab**

[<img class="alignnone wp-image-804 size-medium" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/08/pic5-300x242.png?resize=300%2C242&#038;ssl=1" alt="pic5" width="300" height="242" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/08/pic5.png?resize=300%2C242&ssl=1 300w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/08/pic5.png?w=716&ssl=1 716w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.com/files/2014/08/pic5.png)

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
      Show IM
    </td>
    
    <td>
      ShowIMForLyncMeetings
    </td>
    
    <td>
      0 / 1
    </td>
    
    <td>
      N/A
    </td>
  </tr>
  
  <tr>
    <td>
      Show the Participant List
    </td>
    
    <td>
      ShowRosterForLyncMeetings
    </td>
    
    <td>
      0 / 1
    </td>
    
    <td>
      N/A
    </td>
  </tr>
</table>

&nbsp;