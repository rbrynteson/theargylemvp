---
id: 424
title: 'Resetting default Lync Conference ID&#8217;s'
date: 2013-10-10T17:10:15+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=424
permalink: /2013/10/10/resetting-default-conference-ids/
st_layout_box:
  - right
panels_data:
  - 'a:0:{}'
categories:
  - Lync Server 2013
---
<span style="text-decoration: underline"><em>NOTE: What is suggested in this post is not supported by Microsoft.  I am typically leery of posting these types of solutions but it fixes a specific problem.  Use at your own risk!</em></span>

So over the past seven days I have been ask either directly by customers, through e-mail, on the Microsoft forums or other avenues of how to reset a users default conference ID automatically. Typically when this request is happening it&#8217;s part of a larger issue.  Maybe you changed default SIP domains and removed the old domain.  Maybe you lost your file share and don&#8217;t have a backup to restore conferencing.  Whatever the case is, there may come a time you need to reset everyone&#8217;s ID&#8217;s because you lost conferencing.  I always find people willing to take a little more risk when something is broken beyond repair.

There is no out of the box solution for this.  Users can visit the dial-in page and manually reset the ID&#8217;s.  This works if you have a small problem but if you have lots of people it will become tiresome of support calls walking people through the process.

So what other option do you have? Before we get there, let&#8217;s look at where conferences are stored in database.  In Lync 2013 that is in the front-end server RTCLocal.  For 2010, that is in the back-end database instance.  For the remainder of this conversation we are looking at Lync 2013.

Going to your Lync 2013 RTCLocal SQL instance you will find three databases.  The one we are interested in is the RTC database.  Inside of that are two tables that are helpful &#8211; Resource and Conference. The resource table contains every user who has logged into that server.  Be it internal, external, partners, etc.  I&#8217;m starting here because I want to limit my testing.

So the first thing I need to do is find my resource ID.

SELECT * FROM resource WHERE userAtHost = [&#8216;rbrynteson@avtex.com&#8217;](mailto:'rbrynteson@avtex.com')

Here we can see 293 is my resource ID.

<a href="http://masteringlync.com/2013/10/10/resetting-default-conference-ids/pic1-12/" rel="attachment wp-att-425"><img class="alignnone wp-image-425 size-full" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/10/pic1.png?resize=596%2C218&#038;ssl=1" alt="pic1" width="596" height="218" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/10/pic1.png?w=596&ssl=1 596w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/10/pic1.png?resize=300%2C110&ssl=1 300w" sizes="(max-width: 596px) 100vw, 596px" data-recalc-dims="1" /></a>

So now if I got to the conference table I can get back all of the conferences that are assigned to me:

<a href="http://masteringlync.com/2013/10/10/resetting-default-conference-ids/pic2-9/" rel="attachment wp-att-426"><img class="alignnone wp-image-426 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/10/pic2.png?resize=800%2C218&#038;ssl=1" alt="pic2" width="800" height="218" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/10/pic2.png?w=853&ssl=1 853w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/10/pic2.png?resize=300%2C82&ssl=1 300w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/10/pic2.png?resize=768%2C209&ssl=1 768w" sizes="(max-width: 800px) 100vw, 800px" data-recalc-dims="1" /></a>

Here we can see I have one static (or default conference) and two that are not.  So when I create a new conference I automatically get assigned the conference ID of CDBJ2FMZ.

So when the Outlook client requests a meeting (assuming you are using the default conference ID for meetings), that request is sent to Lync which sends it up to the server.  The server than looks into this table.  If the user has a static (default) conference assigned to them, then it will return that ID to the user.  If there is no static (default) conference, the system will generate a new conference ID, mark it as static and return that to the user.

So if I manually change static from True to False.  We see generating a new meeting request the table changes like the following:<figure id="attachment_430" style="width: 561px" class="wp-caption alignnone">

<a href="http://masteringlync.com/2013/10/10/resetting-default-conference-ids/pic3-8/" rel="attachment wp-att-430"><img class="wp-image-430 size-full" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/10/pic3.png?resize=561%2C138&#038;ssl=1" alt="pic3" width="561" height="138" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/10/pic3.png?w=561&ssl=1 561w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/10/pic3.png?resize=300%2C74&ssl=1 300w" sizes="(max-width: 561px) 100vw, 561px" data-recalc-dims="1" /></a><figcaption class="wp-caption-text">Changed static to false</figcaption></figure> <figure id="attachment_432" style="width: 606px" class="wp-caption alignnone"><a href="http://masteringlync.com/2013/10/10/resetting-default-conference-ids/pic4-7/" rel="attachment wp-att-432"><img class="wp-image-432 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/10/pic4.png?resize=606%2C145&#038;ssl=1" alt="pic4" width="606" height="145" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/10/pic4.png?w=606&ssl=1 606w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/10/pic4.png?resize=300%2C72&ssl=1 300w" sizes="(max-width: 606px) 100vw, 606px" data-recalc-dims="1" /></a><figcaption class="wp-caption-text">Clicked new Lync Meeting in Outlook</figcaption></figure> 

My user has now been assigned a new static (default) conference because of this change.

So what is the end user experience for this?  The answer is it depends.  If the user has already requested a meeting the Outlook client will cache that information.  That is why the first time you request a new meeting each morning (assuming you close Outlook at night) it can take a few seconds but all other Lync meetings happen nearly instantly.  So here are the different scenarios I&#8217;ve tested:

**Outlook Client Open (User has not yet requested a meeting today)**  
The outlook client will reach into the database, find there is no static ID and returns a new static ID to the user by generating a new record.

**Outlook Client Open (User has requested a meeting today)**  
The outlook client will use the cached information and schedule the meeting using the old ID.

**What happens when a person tries to join an existing (old) meeting?**  
Since you have not deleted the old conference, the system will behave like normal and allow you into the conference.  However, if you are doing this type of solution, most likely that old conference was broken and that is why you are doing this.

**What happens if you modify the meeting with the old static ID in Outlook?**  
Outlook when opening the meeting will go and verify the state of the meeting.  Since Outlook believes it&#8217;s the &#8220;default&#8221; meeting it will prompt you that things have changed:<figure id="attachment_433" style="width: 499px" class="wp-caption alignnone">

<a href="http://masteringlync.com/2013/10/10/resetting-default-conference-ids/pic5-6/" rel="attachment wp-att-433"><img class="wp-image-433 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/10/pic5.png?resize=499%2C203&#038;ssl=1" alt="pic5" width="499" height="203" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/10/pic5.png?w=499&ssl=1 499w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/10/pic5.png?resize=300%2C122&ssl=1 300w" sizes="(max-width: 499px) 100vw, 499px" data-recalc-dims="1" /></a><figcaption class="wp-caption-text">Message from Outlook Client</figcaption></figure> 

After clicking OK, the meeting will automatically update to the new static (default) conference and instruct the user to send an update to all participants.

Once you have tested this, you could make the change using a simple SQL Update Query.

Remember, this would never be considered the supported solution but occasionally you have to go outside the box to fix a very broken system.

&nbsp;

&nbsp;