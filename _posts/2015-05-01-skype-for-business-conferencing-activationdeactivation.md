---
id: 1148
title: Skype for Business Conferencing Activation/Deactivation
date: 2015-05-01T20:22:53+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=1148
permalink: /2015/05/01/skype-for-business-conferencing-activationdeactivation/
panels_data:
  - 'a:0:{}'
categories:
  - Skype for Business
---
This is an updated article of my [Lync 2013 Deep Dive](http://masteringlync.com/2014/06/06/conferencing-activationdeactivation-deep-dive/)

**Overview**

Conferences created within Skype for Business don’t last forever.  There is some point in time in which the information needs to be removed from the SQL Database Server to ensure it doesn’t last forever.  The following tables details how long a conference exists within the database.  Some changes have occurred since Lync Server 2013 and those are noted below as well.

We see that meetings expire 365 days (one year) after the last activation (i.e. someone joining them) for single and recurring meetings (with end dates).  There are different behaviors based on ad-hoc and recurring meetings without end dates.  Make sure to read the above article to fully understand those case scenarios.

<table width="100%">
  <tr>
    <td>
    </td>
    
    <td>
      Skype for Business
    </td>
    
    <td>
      Lync Server 2013
    </td>
  </tr>
  
  <tr>
    <td>
      One-Time Scheduled Meeting
    </td>
    
    <td>
      End Date + 365 Days
    </td>
    
    <td>
      End Date + 14 Days
    </td>
  </tr>
  
  <tr>
    <td>
      Recurring Meeting with End Date
    </td>
    
    <td>
      End Date + 365 Days
    </td>
    
    <td>
      End Date + 14 Days
    </td>
  </tr>
  
  <tr>
    <td>
      Recurring Meeting without End Date
    </td>
    
    <td>
      Last Conf + 6 months
    </td>
    
    <td>
    </td>
  </tr>
  
  <tr>
    <td>
      Ad-Hoc Meeting
    </td>
    
    <td>
      8 Hours
    </td>
    
    <td>
      8 Hours
    </td>
  </tr>
</table>

Additionally, we can control the expiration time of meetings using the set-csconferencingconfiguration cmdlet.  You can see here:

<img class="alignnone size-full wp-image-1149" src="https://masteringlync.com/wp-content/uploads/2015/05/1.png?resize=300%2C241&ssl=1 300w" sizes="(max-width: 502px) 100vw, 502px" data-recalc-dims="1" />

NOTE: Remember that 15:00:00 means one second beyond 14 days, 23 hours, 59 minutes, 59 second.  So when we say 14 days, we mean 14 full days.  I’ve seen organizations get caught up on this where there are strict retention periods and they set it wrong.

It&#8217;s important to remember that the content grace period doesn&#8217;t happen until the meeting lifespan has expired.  That means, content for a meeting will last 365 Days + the Content Grace Period above.  Your data, might be sitting on your server for a very long time.

**Activation of a Conference**

This part is pretty simple.  A conference is activated when someone joins it.  When an individual joins a conference, the Focus checks the backend database for the related permissions to the conference.  At that point in time, the Focus communications to the MCU Factory to get the details of available Conferencing Servers and the Focus than starts building those Conferencing Servers for the conference.

All of this information can be found in the database as well.  As soon as a conference is activated, that information is placed into the RTCLocal | RTCDYN | ActiveConference database.  In my screen shot below, you can see that we have five active conferences.

<img class="alignnone size-full wp-image-1150" src="https://masteringlync.com/wp-content/uploads/2015/05/2.png?resize=300%2C51&ssl=1 300w" sizes="(max-width: 697px) 100vw, 697px" data-recalc-dims="1" />

Each column has a very specific purpose.  Here is a quick run down of each column.

  * ConfID: The conference ID is important to note, as it’s a primary key to other tables.
  * ConfStateVersion: The ConfStateVersion is a counter of changes occurring in your meeting.
  * Locked: This is a bit field and tells us if the meeting is locked (True – 1).  A locked meeting will not allow any new participants.
  * AdmissionType: A TinyInt field with a few different options.  
    •    3 – Anyone (No Restrictions)  
    •    2 – Anyone from my organization or the meeting organizer.  
    •    1 – People I Invite
  * AutoPromote: Another TinyInt field.  
    •    0 – People scheduled as presenters  
    •    1 – Anyone from my organization  
    •    2 – Anyone (No Restrictions)
  * PstnLobbyBypass: Exactly what it sounds like.  If set to True (1) than PSTN users get into meetings directly.
  * LastPartID: Not 100% sure what the role of this field is.
  * LastEnterprisePartLeaveTime: Date and time of the last authenticated users to leave a meeting.  This is important later on.
  * ActivationInstance: GUID used by the system.
  * IsLargeMeeting: True (1) or False (0) if it’s a large meeting.

The moral of this section is.  Once a meeting is joined, than we create an instance in the database for an active conference.  There are several moving parts to the creation of a conference.

**Focus** is a SIP endpoint that represents the actual conference in the system.  It’s job is a central gatekeeper.  It’s pretty much responsible for everything for the conference.  From authentication, requesting conferencing servers, etc.

**Focus Factory** handles the logical creation of deletion of conferences for scheduled meetings in the database.

**Conferencing Server Factory** determines the availability and health of the Conferencing Servers in the environment.  During the meeting creation process, it’s responsible for telling the Focus which servers to place which modalities on.  
Here is a great picture buried on one slide that was on the screen for about 7 seconds at Lync Conf 13.

<img class="alignnone size-full wp-image-1151" src="https://masteringlync.com/wp-content/uploads/2015/05/3.png?resize=300%2C239&ssl=1 300w" sizes="(max-width: 704px) 100vw, 704px" data-recalc-dims="1" />

Here we can see how the communication is done between each of the different elements that goes into creating a conference.

**Deactivation of a Conference**

The deactivation of a conference has nothing to do with the expire time we spoke about above.  Instead, the deactivation refers to the act of tearing down a particular instance of a conference.  This is a job of the Focus to monitor activation and deactivation requests.  There are two ways that meetings can be deactivated: Manual and Automatic.

**Manual Deactivation itself can occur three different ways.**

<span style="text-decoration: underline">Presenter Ends the Meeting</span>

This is most likely a rarely used feature as it’s buried under the three dots (more menu) but from here is an option to End Meeting.

<img class="alignnone size-full wp-image-1153" src="https://masteringlync.com/wp-content/uploads/2015/05/5.png?resize=206%2C290&#038;ssl=1" alt="5" width="206" height="290" data-recalc-dims="1" />

When this is done all users are immediately disconnected from the conference.  In the back ground, the client is sending an Exit and End Conference command to the Focus and it terminates all active Conference Servers and disconnects all users.

<span style="text-decoration: underline">Delete Meeting from Outlook</span>

In this case, the organization of the meeting sends a cancellation notice and deletes a meeting.  If users are currently in the meeting, the Focus will immediately delete the meeting from all of the Conference Servers and all users will be removed from the conference.

NOTE: This would not apply to the users’ default meeting space.  If you want to test this you need to make sure to create a unique meeting space.

<span style="text-decoration: underline">User is Deleted from Server</span>

When a user is deleted from Lync, part of that process is to remove any active conferences that might be running in the background, and remove all users.

**Automatic Deactivation can also occur three ways.**

<span style="text-decoration: underline">No Active Participants</span>

I mentioned this before but people most likely rarely use the End Meeting option.  What happens when everyone just leaves a meeting without ending it?  The meeting will remain in the background active.  This is done on the chance that you open up the meeting again immediately.  The exact timer surrounding this one is a bit of a mystery and I couldn’t find any documented values.  Based on my testing, is appears as though the Conferencing Announcement Services (CAS) remains in the conference for 20 minutes.

<span style="text-decoration: underline">90 Minute Rule</span>

I often refer to this as the fantasy football draft rule.  You may recall in our database lookup above, within the ActiveConference table was a LastEnterpriseLeaveTime field.  This field maintains a roster of when the last authenticated user leaves a meeting.  At the 90 minute mark, the Focus will terminate the conference if no enterprise user joined the meeting or if all of the enterprise users have left.  It’s important to note that Federated and PSTN/Anonymous users are not considered enterprise users.

If we look at the SIP Message that is sent at the 90 minute mark, we can clearly see that the Focus is sending a BYE request to the conferencing announcement service (CAS).

<img class="alignnone size-full wp-image-1152" src="https://masteringlync.com/wp-content/uploads/2015/05/4.png?resize=768%2C227&ssl=1 768w" sizes="(max-width: 775px) 100vw, 775px" data-recalc-dims="1" />

<span style="text-decoration: underline">24 Hour – No Join Rule</span>

In order to prevent runaway conferences that are left open forever, the system will terminate conferences that have no new users join in a 24 hour period of time.  That is a long time for a meeting to remain active and a long time for no one new to join your meeting.  In this case, the Focus will terminate the conference with a ‘Conference Terminated – Inactivity’ reason code.