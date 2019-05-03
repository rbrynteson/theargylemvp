---
id: 513
title: The ImAutoArchiving Flag
date: 2013-11-11T20:41:54+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=513
permalink: /2013/11/11/the-imautoarchiving-flag/
st_layout_box:
  - right
categories:
  - Lync Server 2013
---
<a href="http://masteringlync.com/2013/11/11/the-imautoarchiving-flag/pic/" rel="attachment wp-att-514"><img class="size-full wp-image-514 alignright" alt="pic" src="https://i0.wp.com/masteringlync.com/files/2013/11/pic.png?resize=398%2C59&#038;ssl=1" width="398" height="59" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/11/pic.png?w=398&ssl=1 398w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/11/pic.png?resize=300%2C44&ssl=1 300w" sizes="(max-width: 398px) 100vw, 398px" data-recalc-dims="1" /></a>I think the number one request I&#8217;ve seen on the Microsoft forums and by other people is the ability to change/manipulate the &#8220;Save IM conversations in my email Conversation History folder&#8221;.  This can of course be set via the **set-csclientpolicy** but typically this isn&#8217;t the desire of the administrator.  Instead, the administrator would like to blank out (uncheck) the box allow the end-user to login to the client and turn on the feature when/if they want.

If you look at old versions of the client there was a registry key for EnableIMAutoArchiving which could be set that controlled this feature.  However, as of Lync 2013 (and maybe it was true in 2010 also &#8211; don&#8217;t have a client laying around anymore) this registry key has no affect on the behavior of the client.  So that got me thinking: where exactly is this setting stored?

**It&#8217;s in the DB Stupid**

The first test I did was to uncheck this setting and go into the registry to see if I could find any reference for EnableIMAutoArchiving or  IMAutoArchiving but there is none.  So I moved to a second computer and found that when I logged out and back in, the setting was changed on that new computer.  This told me immediately that this setting was being published into the database.

So a quick look in snooper on registration confirms:

<a href="http://masteringlync.com/2013/11/11/the-imautoarchiving-flag/pic2-14/" rel="attachment wp-att-515"><img class="alignnone size-full wp-image-515" alt="pic2" src="https://i2.wp.com/masteringlync.com/files/2013/11/pic22.png?resize=556%2C199&#038;ssl=1" width="556" height="199" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/11/pic22.png?w=556&ssl=1 556w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/11/pic22.png?resize=300%2C107&ssl=1 300w" sizes="(max-width: 556px) 100vw, 556px" data-recalc-dims="1" /></a>

Here we can clearly see that imAutoArchiving is now set to false.  So where exactly in the database is this being set.  Since this is persistent data we know the data will exist in both RTC and RTCLocal (using a standard edition server for testing).  So a quick dip into the database.<figure id="attachment_516" style="width: 522px" class="wp-caption alignnone">

<a href="http://masteringlync.com/2013/11/11/the-imautoarchiving-flag/pic3-14/" rel="attachment wp-att-516"><img class="size-full wp-image-516" alt="pic3" src="https://i1.wp.com/masteringlync.com/files/2013/11/pic33.png?resize=522%2C240&#038;ssl=1" width="522" height="240" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/11/pic33.png?w=522&ssl=1 522w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/11/pic33.png?resize=300%2C138&ssl=1 300w" sizes="(max-width: 522px) 100vw, 522px" data-recalc-dims="1" /></a><figcaption class="wp-caption-text">To discovery the categoryID for otherOptions</figcaption></figure> <figure id="attachment_517" style="width: 539px" class="wp-caption alignnone"><a href="http://masteringlync.com/2013/11/11/the-imautoarchiving-flag/pic4-11/" rel="attachment wp-att-517"><img class="size-full wp-image-517" alt="pic4" src="https://i2.wp.com/masteringlync.com/files/2013/11/pic41.png?resize=539%2C186&#038;ssl=1" width="539" height="186" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/11/pic41.png?w=539&ssl=1 539w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/11/pic41.png?resize=300%2C104&ssl=1 300w" sizes="(max-width: 539px) 100vw, 539px" data-recalc-dims="1" /></a><figcaption class="wp-caption-text">To discovery my resource ID</figcaption></figure> 

We need to know the CategoryID and ResourceID to determine anything.

The CategoryDef table contains all of the published information for a user/system.  Everything from ContactCard, CalendarInformation and more.  We need to find the otherOptions category information.  The Resource table contains every user (internal, external, federated) who has ever touched your system.  Within resource, we need to find my own ID.

The two searches above you can see that they are 21 and 145 respectfully.  So than we go to the PublishedStaticInstance table (which contains everything about the user, contacts, buddy list, etc.) and find this:

<a href="http://masteringlync.com/2013/11/11/the-imautoarchiving-flag/pic5-9/" rel="attachment wp-att-518"><img class="alignnone  wp-image-518" alt="pic5" src="https://i2.wp.com/masteringlync.com/files/2013/11/pic51.png?resize=757%2C241&#038;ssl=1" width="757" height="241" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/11/pic51.png?w=1081&ssl=1 1081w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/11/pic51.png?resize=300%2C95&ssl=1 300w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/11/pic51.png?resize=768%2C244&ssl=1 768w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/11/pic51.png?resize=1024%2C326&ssl=1 1024w" sizes="(max-width: 757px) 100vw, 757px" data-recalc-dims="1" /></a>

There are three rows that are returned for CategoryID 21 (otherOptions).  Each of them contain different information that is available in otherOptions namespace.

**NOTE:** I am casting the [Data] column otherwise it will just display as binary data and you will not be able to actually view the contents of the column.

So if we copy/paste the Data from row 1 into notepad we see this:

<a href="http://masteringlync.com/2013/11/11/the-imautoarchiving-flag/pic6-7/" rel="attachment wp-att-519"><img class="alignnone size-full wp-image-519" alt="pic6" src="https://i2.wp.com/masteringlync.com/files/2013/11/pic61.png?resize=704%2C146&#038;ssl=1" width="704" height="146" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/11/pic61.png?w=704&ssl=1 704w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/11/pic61.png?resize=300%2C62&ssl=1 300w" sizes="(max-width: 704px) 100vw, 704px" data-recalc-dims="1" /></a>

Here we can clearly see the data sitting in the back-end database just asking to be modified.  The problem is, the data is sitting in a Image column type which isn&#8217;t something you can simply run a SQL query against and update.

**Stored Procedures**

Since we cannot edit the information directly (which wouldn&#8217;t be supported but fun to play with in a lab) that means we would need to search through all of the stored procedures and see if we could find anything.  After spending some serious time hunting around I didn&#8217;t find anything that fit the bill.  I&#8217;m sure the procedure is in there somewhere I just couldn&#8217;t find it.

**What is OtherOptions**

There isn&#8217;t really much available on how to update these fields programmatically or direct via SQL.  You can find some data on the Office Dev Center for the oo Namespace here:

http://msdn.microsoft.com/en-us/library/office/dd941485(v=office.13).aspx

But I&#8217;m not a full time developer and not sure how to access this information.  It doesn&#8217;t even specify what programming process (UCWA, SDK, etc.) would be needed if I wanted to write my own app.

**Conclusion**

So in the end, the requested feature of automatically unchecking &#8220;Save IM conversations in my email Conversation History folder&#8221; and then allowing the end-user to turn it on doesn&#8217;t appear to be possible through any hacking of the database or otherwise.  I know this will come as a disappointment to many administrators but understanding where the setting exist should help understand why we can&#8217;t change it directly.

**Unsupported Conclusion**

_Here goes my normal do this only in a lab and I am not at all responsible for what you do if you do this in your production environment._

There is a tool that I found that supports editing Image columns and specifically the XML file within them.  If you download [EMS SQL Studio](http://www.sqlmanager.net/en/products/studio/mssql) (I&#8217;ve never used this product before &#8211; just found it on the innerwebs) you can see and modify the XML in the data column.  Here you can see my XML file:

<a href="http://masteringlync.com/2013/11/11/the-imautoarchiving-flag/pic8-3/" rel="attachment wp-att-524"><img class="alignnone  wp-image-524" alt="pic8" src="https://i0.wp.com/masteringlync.com/files/2013/11/pic8.png?resize=592%2C337&#038;ssl=1" width="592" height="337" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/11/pic8.png?w=658&ssl=1 658w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/11/pic8.png?resize=300%2C171&ssl=1 300w" sizes="(max-width: 592px) 100vw, 592px" data-recalc-dims="1" /></a>

I can edit the imAutoArchiving row from true to false and upon logout/login of my Lync client the setting has been changed.  Again, direct editing of the database is NEVER supported.  But maybe a SQL DBA can use this knowledge to find the stored procedure that is called.