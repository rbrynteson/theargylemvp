---
id: 765
title: Conferencing Activation/Deactivation Deep Dive
date: 2014-06-06T17:06:28+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=765
permalink: /2014/06/06/conferencing-activationdeactivation-deep-dive/
st_layout_box:
  - right
panels_data:
  - 'a:0:{}'
categories:
  - Lync Server 2013
---
Recently was asked the question: how long will this meeting last if I log out?  It&#8217;s a common question I&#8217;ve seen and although I knew the answer, I asked myself, how well documented is the Lync Conference tear down behavior.  So I did some quick searching and found only this one <a href="http://www.justin-morris.net/lync-conference-expiration-and-deactivation-explained/" target="_blank">article by Justin Morris</a>.

<span style="font-size: medium"><strong>Overview</strong></span>

The above article does a good job of explaining what happens.  We see that meetings expire 14 days after the last activation (i.e. someone joining them) for single and recurring meetings (with end dates).  There are different behaviors based on ad-hoc and recurring meetings without end dates.  Make sure to read the above article to fully understand those case scenarios.

Additionally, we can control the expire time of meetings using the set-csconferencingconfiguration cmdlet.  You can see here:

[<img class="alignnone wp-image-766 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/06/pic0.png?resize=511%2C417&#038;ssl=1" alt="pic0" width="511" height="417" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/06/pic0.png?w=511&ssl=1 511w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/06/pic0.png?resize=300%2C245&ssl=1 300w" sizes="(max-width: 511px) 100vw, 511px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.com/files/2014/06/pic0.png)

NOTE: Remember that 15:00:00 means one second beyond 14 days, 23 hours, 59 minutes, 59 second.  So when we say 14 days, we mean 14 full days.  I&#8217;ve seen organizations get caught up on this where there are strict retention periods and they set it wrong.

With all of that said, I want to know the why and how so lets dig into a few thing.

<span style="font-size: medium"><strong>Activation of a Conference</strong></span>

This part is pretty simple.  A conference is activated when someone joins it.  When an individual joins a conference, the Focus checks the backend database for the related permissions to the conference.  At that point in time, the Focus communications to the MCU Factory to get the details of available Conferencing Servers and the Focus than starts building those Conferencing Servers for the conference.

All of this information can be found in the database as well.  As soon as a conference is activated, that information is placed into the RTCLocal | RTCDYN | ActiveConference database.  In my screen shot below, you can see that we have five active conferences.

[<img class="alignnone wp-image-767" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/06/pic1-1024x176.png?resize=700%2C120&#038;ssl=1" alt="pic1" width="700" height="120" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/06/pic1.png?resize=1024%2C176&ssl=1 1024w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/06/pic1.png?resize=300%2C51&ssl=1 300w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/06/pic1.png?resize=768%2C132&ssl=1 768w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/06/pic1.png?w=1102&ssl=1 1102w" sizes="(max-width: 700px) 100vw, 700px" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.com/files/2014/06/pic1.png)

There are a few interesting items of note in table.  (SIDE NOTE: I started a Lync Data Dictionary for the SQL database years ago and never finished the project.  People should tell me if there is any interest in that since Microsoft doesn&#8217;t appear willing to share theirs.  🙁 )

  * ConfID: The conference ID is important to note, as it&#8217;s a primary key to other tables.
  * ConfStateVersion: The ConfStateVersion is a counter of changes occurring in your meeting.
  * Locked: This is a bit field and tells us if the meeting is locked (True &#8211; 1).  A locked meeting will not allow any new participants.
  * AdmissionType: A TinyInt field with a few different options. 
      * 3 &#8211; Anyone (No Restrictions)
      * 2 &#8211; Anyone from my organization or the meeting organizer.
      * 1 &#8211; People I Invite
  * AutoPromote: Another TinyInt field. 
      * 0 &#8211; People scheduled as presenters
      * 1 &#8211; Anyone from my organization
      * 2 &#8211; Anyone (No Restrictions)
  * PstnLobbyBypass: Exactly what it sounds like.  If set to True (1) than PSTN users get into meetings directly.
  * LastPartID: Not 100% sure what the role of this field is.
  * LastEnterprisePartLeaveTime: Date and time of the last authenticated users to leave a meeting.  This is important later on.
  * ActivationInstance: GUID used by the system.
  * IsLargeMeeting: True (1) or False (0) if it&#8217;s a large meeting.

The moral of this section is.  Once a meeting is joined, than we create an instance in the database for an active conference.  There are several moving parts to the creation of a conference.

<span style="text-decoration: underline">Focus</span> is a SIP endpoint that represents the actual conference in the system.  It&#8217;s job is a central gatekeeper.  It&#8217;s pretty much responsible for everything for the conference.  From authentication, requesting conferencing servers, etc.

<span style="text-decoration: underline">Focus Factory</span> handles the logical creation of deletion of conferences for scheduled meetings in the database.

<span style="text-decoration: underline">Conferencing Server Factory </span>determines the availability and health of the Conferencing Servers in the environment.  During the meeting creation process, it&#8217;s responsible for telling the Focus which servers to place which modalities on.

Here is a great picture buried on one slide that was on the screen for about 7 seconds at Lync Conf 13.

[<img class="alignnone wp-image-770 size-full" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/06/pic4.png?resize=727%2C583&#038;ssl=1" alt="pic4" width="727" height="583" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/06/pic4.png?w=727&ssl=1 727w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/06/pic4.png?resize=300%2C241&ssl=1 300w" sizes="(max-width: 727px) 100vw, 727px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.com/files/2014/06/pic4.png)

Here we can see how the communication is done between each of the different elements that goes into creating a conference.

<span style="font-size: medium"><strong>Deactivation of a Conference</strong></span>

The deactivation of a conference has nothing to do with the expire time we spoke about above.  Instead, the deactivation refers to the act of tearing down a particular instance of a conference.  This is a job of the Focus to monitor activation and deactivation requests.  There are two ways that meetings can be deactivated: Manual and Automatic.

**<span style="font-size: small">Manual Deactivation</span>** itself can occur three different ways.

<span style="text-decoration: underline">Presenter Ends the Meeting</span>

This is most likely a rarely used feature as it&#8217;s buried under the three dots (more menu) but from here is an option to End Meeting.

[<img class="alignnone wp-image-768 size-full" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/06/pic2.png?resize=229%2C307&#038;ssl=1" alt="pic2" width="229" height="307" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/06/pic2.png?w=229&ssl=1 229w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/06/pic2.png?resize=224%2C300&ssl=1 224w" sizes="(max-width: 229px) 100vw, 229px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.com/files/2014/06/pic2.png)

When this is done all users are immediately disconnected from the conference.  In the back ground, the client is sending an Exit and End Conference command to the Focus and it terminates all active Conference Servers and disconnects all users.

<span style="text-decoration: underline">Delete Meeting from Outlook</span>

In this case, the organization of the meeting sends a cancellation notice and deletes a meeting.  If users are currently in the meeting, the Focus will immediately delete the meeting from all of the Conference Servers and all users will be removed from the conference.

NOTE: This would not apply to the users default meeting space.  If you want to test this you need to make sure to create a unique meeting space.

<span style="text-decoration: underline">User is Deleted from Server</span>

When a user is deleted from Lync, part of that process is to remove any active conferences that might be running in the background, and remove all users.

**Automatic Deactivation** can also occur three ways.

<span style="text-decoration: underline">No Active Participants</span>

I mentioned this before but people most likely rarely use the End Meeting option.  What happens when everyone just leaves a meeting without ending it?  The meeting will remain in the background active.  This is done on the chance that you open up the meeting again immediately.  The exact timer surrounding this one is a bit of a mystery and I couldn&#8217;t find any documented values.  Based on my testing, is appears as though the Conferencing Announcement Services (CAS) remains in the conference for 20 minutes.

<span style="text-decoration: underline">90 Minute Rule</span>

I often refer to this as the fantasy football rule.  You may recall in our database lookup above, within the ActiveConference table was a LastEnterpriseLeaveTime field.  This field maintains a roster of when the last authenticated user leaves a meeting.  At the 90 minute mark, the Focus will terminate the conference if no enterprise user joined the meeting or if all of the enterprise users have left.  It&#8217;s important to note that Federated and PSTN/Anonymous users are not considered enterprise users.

Why call it the fantasy football rule?  90 minutes is just long enough to hold your draft.  🙂

If we look at the SIP Message that is sent at the 90 minute mark, we can clearly see that the Focus is sending a BYE request to the conferencing announcement service (CAS).

[<img class="alignnone wp-image-769 size-full" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/06/pic3.png?resize=796%2C221&#038;ssl=1" alt="pic3" width="796" height="221" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/06/pic3.png?w=796&ssl=1 796w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/06/pic3.png?resize=300%2C83&ssl=1 300w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/06/pic3.png?resize=768%2C213&ssl=1 768w" sizes="(max-width: 796px) 100vw, 796px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.com/files/2014/06/pic3.png)

<span style="text-decoration: underline">24 Hour &#8211; No Join Rule</span>

In order to prevent runaway conferences that are left open forever, the system will terminate conferences that have no new users join in a 24 hour period of time.  That is a long time for a meeting to remain active and a long time for no one new to join your meeting.  In this case, the Focus will terminate the conference with a &#8216;Conference Terminated &#8211; Inactivity&#8217; reason code.

**Conclusion**

There you have it.  The ins and outs of the conference activation and deactivation process.  Maybe in a future post I&#8217;ll break down the C3P requests between each element for some super geeky fun but that is all for today.