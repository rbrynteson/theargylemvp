---
id: 593
title: Client Registry Keys (Part 3)
date: 2014-02-26T01:59:23+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=593
permalink: /2014/02/26/client-registry-keys-part-3/
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

**My Picture Tab**

[<img class="alignnone  wp-image-594" alt="mypic" src="https://i0.wp.com/masteringlync.com/files/2013/12/mypic.png?resize=511%2C417&#038;ssl=1" width="511" height="417" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/12/mypic.png?w=730&ssl=1 730w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/12/mypic.png?resize=300%2C245&ssl=1 300w" sizes="(max-width: 511px) 100vw, 511px" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.com/files/2013/12/mypic.png)

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
      Hide my picture
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
      Show my picture
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

All of the picture settings appear to be stored in the database.  I&#8217;ll go hunting for those at a later time.

You can control some of this via client policy.  You can use the DisplayPhoto policy with these settings.

NoPhoto &#8211; Photos are not displayed in Lync 2013.

PhotosFromADOnly &#8211; Only photos that have been published in Active directory.

AllPhotos &#8211; Photos from AD or URL are disabled.

**Phones Tab**

[<img class="alignnone  wp-image-595" alt="pic2" src="https://i0.wp.com/masteringlync.com/files/2013/12/pic2.png?resize=510%2C417&#038;ssl=1" width="510" height="417" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/12/pic2.png?w=729&ssl=1 729w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/12/pic2.png?resize=300%2C245&ssl=1 300w" sizes="(max-width: 510px) 100vw, 510px" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.com/files/2013/12/pic2.png)

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
      My Phone Numbers
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
      Turn on TTY Mode
    </td>
    
    <td>
      <span style="color: #000000">EnableTTY</span>
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
      Join meeting audio from
    </td>
    
    <td>
      <span style="color: #000000">JoinAudioConferenceFrom</span>
    </td>
    
    <td>
      0/1/2 &#8230; *
    </td>
    
    <td>
      N/A
    </td>
  </tr>
  
  <tr>
    <td>
      Before I join meetings&#8230;
    </td>
    
    <td>
      AllowOverridingDeviceAtJoinTime
    </td>
    
    <td>
      0 &#8211; Automatically Join<br /> 1 &#8211; Prompt on Join
    </td>
    
    <td>
      N/A
    </td>
  </tr>
</table>

* 0 = Do not join audio.  1 = Lync. 2 and higher are other numbers listed. For my client 2 is work and 3 is mobile.

&nbsp;