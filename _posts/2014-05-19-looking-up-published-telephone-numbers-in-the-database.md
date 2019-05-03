---
id: 749
title: Looking up published telephone numbers in the database.
date: 2014-05-19T13:03:30+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=749
permalink: /2014/05/19/looking-up-published-telephone-numbers-in-the-database/
st_layout_box:
  - right
panels_data:
  - 'a:0:{}'
categories:
  - Lync Server 2013
---
Occasionally you run into a situation where you view an individuals contact card and the phone number is wrong:<figure id="attachment_750" style="width: 452px" class="wp-caption alignnone">

[<img class="wp-image-750 size-full" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/05/pic11.png?resize=452%2C342&#038;ssl=1" alt="pic1" width="452" height="342" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/05/pic11.png?w=452&ssl=1 452w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/05/pic11.png?resize=300%2C227&ssl=1 300w" sizes="(max-width: 452px) 100vw, 452px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.com/files/2014/05/pic11.png)<figcaption class="wp-caption-text">Phone number is actually x4150</figcaption></figure> 

However, when we view the individuals information in Active Directory, we have the following:

[<img class="alignnone wp-image-751 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/05/pic21.png?resize=525%2C84&#038;ssl=1" alt="pic2" width="525" height="84" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/05/pic21.png?w=525&ssl=1 525w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/05/pic21.png?resize=300%2C48&ssl=1 300w" sizes="(max-width: 525px) 100vw, 525px" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.com/files/2014/05/pic21.png)

And if we dump the address book file to text, we clearly see that the phone number has been normalized as planned as defined in the Company\_Phone\_Number\_Normalization\_Rules.txt file.

So the question are: where is this number coming from and how do we update it?

**Hacking the Database**

The first thing we need to realize is that the phone numbers that appear on the contact card can come from multiple locations and one of those is the users published information in the database.

All of the database queries are going to be run against the RTCLocal database.  So if this is a Standard Edition deployment, it&#8217;s simply, it&#8217;s your only front-end server.  If it&#8217;s an Enterprise Deployment, you will first need to find the primary server for that users routing group.  That will add a little complexity to this hunt.

So where can we find that information:

<span style="text-decoration: underline"><em>CategoryDef Table</em></span>

The CategoryDef table contains a list of published information.  One of those items will be UserInformation:

<p style="padding-left: 30px">
  <em>/****** Script for SelectTopNRows command from SSMS  ******/</em><br /> <em>SELECT TOP 1000 [CategoryId],[Name],[Private],[ReplicationFlags],[QuotaTotalSize] FROM [rtc].[dbo].[CategoryDef]</em>
</p>

Here we can see a result of the query:

[<img class="alignnone wp-image-752 size-full" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/05/pic3.png?resize=681%2C400&#038;ssl=1" alt="pic3" width="681" height="400" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/05/pic3.png?w=681&ssl=1 681w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/05/pic3.png?resize=300%2C176&ssl=1 300w" sizes="(max-width: 681px) 100vw, 681px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.com/files/2014/05/pic3.png)

UserInformation should be CategoryID 23 but you should verify just in case.

<span style="text-decoration: underline"><em>Resource Table</em></span>

The second item that needs to be found is the resource ID for the user we are looking for.

<p style="padding-left: 30px">
  <em>/****** Script for SelectTopNRows command from SSMS  ******/</em><br /> <em>SELECT TOP 1000 [ResourceId] ,[UserAtHost] FROM [rtc].[dbo].[Resource] WHERE UserAtHost LIKE &#8216;sarah@%&#8217;</em>
</p>

Here, we are searching for any resource with the SIP address that starts with [&#8216;sarah@&#8217;](mailto:'sarah@').  The result returns ResourceID 1384.

<span style="text-decoration: underline"><em>PublishedStaticInstance Table</em></span>

In this table, we will find all of the published information for all users based on the above category IDs and presence containers.  The following query will narrow this down to the specific user and phone number information:

<p style="padding-left: 30px">
  <em>/****** Script for SelectTopNRows command from SSMS  ******/</em><br /> <em>SELECT TOP 1000 [PublisherId],[CategoryId],[ContainerNum],cast(cast([Data] as varbinary(max)) as varchar(max)) as Data FROM [rtc].[dbo].[PublishedStaticInstance] WHERE PublisherId = &#8216;1384&#8217; AND [CategoryId] = &#8217;23&#8217;</em>
</p>

Where CategoryID is equal to the category for UserInformation and PublisherID is equal to the ResourceID found above.  You can see that we are casting the [Data] column to varchar so we can easily read it.  When we copy the results from the table to notepad, we get the following results:

[<img class="alignnone wp-image-753 size-medium" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/05/pic4-300x94.png?resize=300%2C94&#038;ssl=1" alt="pic4" width="300" height="94" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/05/pic4.png?resize=300%2C94&ssl=1 300w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/05/pic4.png?resize=768%2C240&ssl=1 768w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/05/pic4.png?w=940&ssl=1 940w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.com/files/2014/05/pic4.png)

Here, we can clearly see that the phone number that has been published, is incorrect.

**How Does It Get There?**

When a user logs into their client, their phone number information can be found on the Tools | Options | Phones tab of the Lync Client.  This information is populated based on information stored in Active Directory and using the rules specified in Company\_Phone\_Number\_Normalization\_Rules.txt we normalize that number.  The user than publishes that information into the SQL database for themselves.  So that means:

1. It&#8217;s the users client that is responsible for updating the database.  This can be a problem if the user rarely logs into their Lync client (executives sometimes will log in rarely) or use certain 3PIP phones only.

2. The client has issues updating the local address book copy.

(NOTE: As of this writing, there is a current bug with Lync 2013 Enterprise Pools where a delta address book isn&#8217;t created correctly.  This can lead to a significant delay in updating the local address book file on the local machine.  Which means, it can delay the updating of published information in the database as well.  There are two workarounds: First, you can go to WebSearchOnly in the client policy.  Second, you can change the address book configuration.  You can find the details <a href="http://www.ucryan.com/2014/03/lync-2013-server-fails-to-update.html" target="_blank">here</a>.)

So when fighting contact card information you need to:

1. Ensure you have all of your normalization files in place.  Especially check the Company\_Phone\_Number\_Normalization\_Rules for any mistakes.

2. Troubleshoot from the users computer who is showing the wrong number as they are the ones actually publishing the incorrect number.  Start by deleting galcontacts.db and forcing the client to download a new copy of the address book.  Remember, you can control the delay via the registry.

**One Last Number**

There is one last number you should be aware of.  When you hover over a contacts phone icon in the Lync Client you will see this:

[<img class="alignnone wp-image-755 size-medium" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/05/pic5-300x187.png?resize=300%2C187&#038;ssl=1" alt="pic5" width="300" height="187" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/05/pic5.png?resize=300%2C187&ssl=1 300w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/05/pic5.png?w=415&ssl=1 415w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.com/files/2014/05/pic5.png)

There are two items at play here.  First, the registry has an entry for the last number you called for that contact.  You can check out my old article on [controlling click-to-call](http://masteringlync.com/2014/01/27/controlling-click-to-call/) for more information.  Second, the Lync client also caches telephone information in the SIP_ folder.  If you browse to the C:Users<username>AppDataLocalMicrosoftOffice15.0Lyncsip_user@domain.com folder, you will find the CoreContact.cache file.

[<img class="alignnone wp-image-756 size-medium" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/05/pic6-300x33.jpg?resize=300%2C33&#038;ssl=1" alt="pic6" width="300" height="33" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/05/pic6.jpg?resize=300%2C33&ssl=1 300w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/05/pic6.jpg?resize=768%2C83&ssl=1 768w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/05/pic6.jpg?w=821&ssl=1 821w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.com/files/2014/05/pic6.jpg)

If you delete the CoreContact.cache file, the phone number displayed when hovering over the telephone icon will be pulled from the published information and displayed.  You will also see the cache file immediately recreated with this newly discovered information.