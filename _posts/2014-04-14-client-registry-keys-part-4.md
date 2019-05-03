---
id: 697
title: Client Registry Keys (Part 4)
date: 2014-04-14T15:16:48+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=697
permalink: /2014/04/14/client-registry-keys-part-4/
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

**Alerts Tab**

Make sure to check out the Part One for the location of the registry key otherwise many of these items won&#8217;t make a ton of sense.

[<img class="alignnone wp-image-700 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/04/pic1.png?resize=800%2C696&#038;ssl=1" alt="pic1" width="800" height="696" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/04/pic1.png?w=862&ssl=1 862w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/04/pic1.png?resize=300%2C261&ssl=1 300w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/04/pic1.png?resize=768%2C668&ssl=1 768w" sizes="(max-width: 800px) 100vw, 800px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.com/files/2014/04/pic1.png)

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
      Tell me when someone adds me to his or her contact list
    </td>
    
    <td>
      N/A
    </td>
    
    <td>
      N/A
    </td>
    
    <td>
      EnableNotificationForNewSubscribers
    </td>
  </tr>
  
  <tr>
    <td>
      When my status is Do Not Disturb
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
      Contacts not using Lync
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
      Allow invites from domains my admin hasn&#8217;t verified
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

NOTE: All of these settings are stored in the database for the user and follow them from machine to machine.  For example, reviewing the XML as part of the client login process we find:

[<img class="alignnone wp-image-704 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/04/pic4.png?resize=800%2C122&#038;ssl=1" alt="pic4" width="800" height="122" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/04/pic4.png?w=813&ssl=1 813w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/04/pic4.png?resize=300%2C46&ssl=1 300w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/04/pic4.png?resize=768%2C117&ssl=1 768w" sizes="(max-width: 800px) 100vw, 800px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.com/files/2014/04/pic4.png)

This is the setting for Allow Invites from Domains my admin hasn&#8217;t verified.  Likewise, here are the settings for When my **Status is Do Not Disturb**:

[<img class="alignnone wp-image-705 size-medium" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/04/pic5-300x84.png?resize=300%2C84&#038;ssl=1" alt="pic5" width="300" height="84" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/04/pic5.png?resize=300%2C84&ssl=1 300w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/04/pic5.png?w=669&ssl=1 669w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.com/files/2014/04/pic5.png)

As for &#8220;editing&#8221; these features.  You can&#8217;t of course modify these directly in the database as everything is stored in XML documents.  You can jump over to my article on the [IMAutoArchivingFlag](http://masteringlync.com/2013/11/11/the-imautoarchiving-flag/) which goes more in-depth on this and does show an unsupported method to edit.

**Persistent Chat Tab**

[<img class="alignnone wp-image-701 size-full" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/04/pic2.png?resize=800%2C696&#038;ssl=1" alt="pic2" width="800" height="696" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/04/pic2.png?w=862&ssl=1 862w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/04/pic2.png?resize=300%2C261&ssl=1 300w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/04/pic2.png?resize=768%2C668&ssl=1 768w" sizes="(max-width: 800px) 100vw, 800px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.com/files/2014/04/pic2.png)

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
      New Message: Show the message in a new window
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
      New Message: Show me an alert
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
      New Message: Play this sound
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
      High Priority: Show the message in a new window
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
      High Priority: Show me an alert
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
      High Priority: Play this sound
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

NOTE: All of these settings are stored in the database for the user and follow them from machine to machine.

**Ringtones and Sounds Tab**

[<img class="alignnone wp-image-702 size-full" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/04/pic3.png?w=800&#038;ssl=1" alt="pic3" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/04/pic3.png?w=863&ssl=1 863w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/04/pic3.png?resize=300%2C261&ssl=1 300w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/04/pic3.png?resize=768%2C667&ssl=1 768w" sizes="(max-width: 800px) 100vw, 800px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.com/files/2014/04/pic3.png)

<table width="100%">
  <tr>
    <td style="width: 200px">
      Description
    </td>
    
    <td>
      Reg Key
    </td>
    
    <td style="width: 100px">
      Values
    </td>
    
    <td style="width: 100px">
      Client Policy
    </td>
  </tr>
  
  <tr>
    <td>
      Calls To: My work number
    </td>
    
    <td>
      HKEY_CURRENT_USER\AppEvents\Schemes\Apps\Communicator\ LYNC_ringing.Current
    </td>
    
    <td>
      String to File Path *
    </td>
    
    <td>
      N/A
    </td>
  </tr>
  
  <tr>
    <td>
      Calls To: My team
    </td>
    
    <td>
      HKEY_CURRENT_USER\AppEvents\Schemes\Apps\Communicator\ LYNC_incomingteamcall.Current
    </td>
    
    <td>
      String to File Path *
    </td>
    
    <td>
      N/A
    </td>
  </tr>
  
  <tr>
    <td>
      Calls To: People I manage calls for
    </td>
    
    <td>
      HKEY_CURRENT_USER\AppEvents\Schemes\Apps\Communicator\ LYNC_incomingdelegatecall.Current
    </td>
    
    <td>
      String to File Path *
    </td>
    
    <td>
      N/A
    </td>
  </tr>
  
  <tr>
    <td>
      Calls To: My Response Group
    </td>
    
    <td>
      HKEY_CURRENT_USER\AppEvents\Schemes\Apps\Communicator\ LYNC_incomingteamcall.Current
    </td>
    
    <td>
      String to File Path *
    </td>
    
    <td>
      N/A
    </td>
  </tr>
  
  <tr>
    <td>
      Play sounds in Lync
    </td>
    
    <td>
      playSoundFeedback **
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
      Mute incoming IM alert sounds when viewing an IM conversation
    </td>
    
    <td>
      suspendSoundWhenConversationWindowInForeground
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
      Keep sounds to a minimum when my status is Busy
    </td>
    
    <td>
      suspendSoundWhenBusy
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
      Keep sounds to a minimum when my status is Do Not Disturb
    </td>
    
    <td>
      suspendSoundWhenDND
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
      Play music on hold
    </td>
    
    <td>
      MusicOnHoldDisabled
    </td>
    
    <td>
      0/1
    </td>
    
    <td>
      EnableClientMusicOnHold
    </td>
  </tr>
  
  <tr>
    <td>
      MOH File Path
    </td>
    
    <td>
      MusicOnHoldAudioFile
    </td>
    
    <td>
      String
    </td>
    
    <td>
      MusicOnHoldAudioFile
    </td>
  </tr>
</table>

&nbsp;

NOTE: Remember that setting policies via In-Band Client Policy will override all registry settings.

* File path default is here: C:Program Files (x86)Microsoft OfficeOffice15Media

  * LYNC_ringing.wav &#8211; Wayfarer
  * LYNC_ringtone2.wav &#8211; Duotone
  * LYNC_ringtone3.wav &#8211; InMotion
  * LYNC_ringtone4.wav &#8211; LowKeyWave
  * LYNC_ringtone5.wav &#8211; PureTone
  * LYNC_ringtone6.wav &#8211; SilverMallets
  * LYNC_ringtone7.wav &#8211; VintagePulse

** Must be set to true (1) in order to use any of the suspendSound options.

\*** There is no GUI option for this but you can control the private line inbound calls here: LYNC_incomingprivatelinecall

&nbsp;

&nbsp;