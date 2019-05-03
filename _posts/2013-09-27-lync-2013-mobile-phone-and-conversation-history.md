---
id: 394
title: Lync 2013 Mobile Phone and Conversation History
date: 2013-09-27T03:38:50+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=394
permalink: /2013/09/27/lync-2013-mobile-phone-and-conversation-history/
st_layout_box:
  - right
categories:
  - Lync Server 2013
---
So over the past week I&#8217;ve been asked by two different clients about conversation history on the Lync Mobile Client.  And then the same conversation came up via an internal distribution list and I decided it was a good time to do a little research to answer this simple question:

Does the Lync 2013 Mobile Client honor retention policies for IM&#8217;s?<figure id="attachment_395" style="width: 222px" class="wp-caption alignright">

<a href="http://masteringlync.com/2013/09/27/lync-2013-mobile-phone-and-conversation-history/wp_ss_20130926_0001/" rel="attachment wp-att-395"><img class="wp-image-395 " style="border: 1px solid black" alt="wp_ss_20130926_0001" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/09/wp_ss_20130926_0001.jpg?resize=222%2C369&#038;ssl=1" width="222" height="369" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/09/wp_ss_20130926_0001.jpg?w=768&ssl=1 768w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/09/wp_ss_20130926_0001.jpg?resize=180%2C300&ssl=1 180w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/09/wp_ss_20130926_0001.jpg?resize=614%2C1024&ssl=1 614w" sizes="(max-width: 222px) 100vw, 222px" data-recalc-dims="1" /></a><figcaption class="wp-caption-text">Test #1</figcaption></figure> 

It is very common in large enterprise organizations to disable two of my favorite client features &#8211; Auto Archiving of IMs to Outlook & Auto Archiving of Call Logs to Outlook.  The decision to turn these features off is typically pushed in legal and HR departments.  So let&#8217;s take a look at what happens with the different features enabled and disabled.  We will be playing with two primary client policy options.

  * EnableIMAutoArchiving
  * DisableSavingIM

For all of these tests one user is logged in via the full Lync 2013 client running the September 2013 patch.  The second user is running the latest version of the Lync 2013 Mobile Client on Windows Phone 8.

**Test #1**

  * EnableIMAutoArchiving = $Null
  * DisableSavingIM = $Null

This is the easiest configuration for me to test as this is the default configuration of Lync 2013 out of the box.  So checking on my phone I clearly see that my conversation history is being saved.

**Test #2**

  * EnableIMAutoArchiving = False
  * DisableSavingIM = $Null

This is where the mobile client does some very interesting and fun things.  The start of a conversation does exactly what you would expect it.  In the first screen shot (below left) we see a conversation on the device.  The second screen shot is more interesting.  Because the mobile device is honoring the no-archiving of messages the moment the Lync conversation IM window is closed on the desktop client the conversation disappears immediately from the mobile phone.<figure id="attachment_396" style="width: 222px" class="wp-caption alignleft">

<a href="http://masteringlync.com/2013/09/27/lync-2013-mobile-phone-and-conversation-history/wp_ss_20130926_0002/" rel="attachment wp-att-396"><img class="wp-image-396 " style="border: 1px solid black" alt="wp_ss_20130926_0002" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/09/wp_ss_20130926_0002.jpg?resize=222%2C369&#038;ssl=1" width="222" height="369" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/09/wp_ss_20130926_0002.jpg?w=768&ssl=1 768w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/09/wp_ss_20130926_0002.jpg?resize=180%2C300&ssl=1 180w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/09/wp_ss_20130926_0002.jpg?resize=614%2C1024&ssl=1 614w" sizes="(max-width: 222px) 100vw, 222px" data-recalc-dims="1" /></a><figcaption class="wp-caption-text">Test #2 &#8211; Conversation Started</figcaption></figure> <figure id="attachment_397" style="width: 222px" class="wp-caption alignleft"><a href="http://masteringlync.com/2013/09/27/lync-2013-mobile-phone-and-conversation-history/wp_ss_20130926_0003/" rel="attachment wp-att-397"><img class="wp-image-397 " style="border: 1px solid black" alt="wp_ss_20130926_0003" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/09/wp_ss_20130926_0003.jpg?resize=222%2C369&#038;ssl=1" width="222" height="369" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/09/wp_ss_20130926_0003.jpg?w=768&ssl=1 768w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/09/wp_ss_20130926_0003.jpg?resize=180%2C300&ssl=1 180w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/09/wp_ss_20130926_0003.jpg?resize=614%2C1024&ssl=1 614w" sizes="(max-width: 222px) 100vw, 222px" data-recalc-dims="1" /></a><figcaption class="wp-caption-text">Test #2 &#8211; Conversation Windows Closed</figcaption></figure> 

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

So in this configuration we have obtained our desired result of not saving the conversation.  However this configuration poses one significant problem for your mobile users.  What happens if the desktop user happens to simply close the IM window.  Unlike two desktop users, even if user A closed the IM window, the IM window would remain open for user B.  For the mobile user, they would have absolutely no idea the conversation was started.

**Test #3**

  * EnableIMAutoArchiving = $Null
  * DisableSavingIM = True

In this test we reverse the IMAutoArchiving and SavingIM feature.  This scenario would not meet the company requirements outlined above but curious if anything would be different.  In this test scenario the Lync 2013 Mobile Client behaves exactly the same as Test #2.  I went ahead and tried

**Test #4** (EnableIMAutoArchiving = False, DisableSavingIM = True) assuming the result is exactly the same.  As predicted the client did not save the IM history but still makes conversations disappear once the far user closes the IM window.

This problem is exacerbated if the Lync client isn&#8217;t in the foreground.  The mobile phone will display the notification on the phone for only a matter of seconds.  Once the notification has disappeared, the mobile user is left with a notification on the live tile that a missed conversation occurred but when you enter the Lync client you do not see who the IM was from.

**Test #5**

This is a set of tests but I used an iOS device for the mobile device to see if different behavior is noted.  Unfortunately, the process in every test scenario turned out exactly the same.  The only difference I noticed was that on the iOS device when the Lync client is not in the foreground, when you launch the application you see the notification for a split second again.  So if you are lucky, you can see who the person was who sent the original IM, but you will not be able open the conversation.

I wasn&#8217;t able to repeat these tests on an Android device as I do not have one available but I see no reason why they would behave differently.