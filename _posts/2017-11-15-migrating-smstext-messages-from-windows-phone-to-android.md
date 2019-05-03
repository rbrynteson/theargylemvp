---
id: 1417
title: Migrating SMS/Text Messages from Windows Phone to Android
date: 2017-11-15T17:58:04+00:00
author: Richard Brynteson
excerpt: 'The search is over!  Learn how to migrate all of your Windows Phone SMS messages to your new Android Device.'
layout: post
guid: http://masteringlync.com/?p=1417
permalink: /2017/11/15/migrating-smstext-messages-from-windows-phone-to-android/
nkweb_code_in_head:
  - default
nkweb_Use_Custom_js:
  - default
nkweb_Custom_js:
  - ""
nkweb_Use_Custom_Values:
  - default
nkweb_Custom_Values:
  - ""
nkweb_Use_Custom:
  - 'false'
nkweb_Custom_Code:
  - ""
colormag_page_layout:
  - default_layout
image: https://masteringlync.com/wp-content/uploads/sites/2/2017/11/Andy-Rubinu2019s-Essential-PH-1-smartphone.jpg
categories:
  - Essential PH-1
---
So it&#8217;s official, I&#8217;m no longer using a Windows Phone.  My trusty and wonderful Lumia 950 has been having all sorts of ear piece and ringer issues for the past two months and I&#8217;ve finally gotten fed up with missing notifications and calls.  So this is well outside the Lync/Skype/Teams world but maybe someone else will find this handy so here goes:

**Step 1:** Download and install the <a href="https://www.microsoft.com/en-us/store/p/wp-message-backup/9nblggh2srtp" target="_blank" rel="noopener">WP Message Backup </a>from the Windows Store and install it on your Windows Phone.  The UI is a bit of a mess but once you understand what is happening then it&#8217;s not too bad.  The first step is to add your phone.  So select Windows 10 Mobile.  It will ask you to login to OneDrive.

IMPORTANT: You must be using the OneDrive backup for your messages.  So from either the default SMS/Text application or from the new Skype app, ensure Sync Messages Between Devices is set to On.  I would suggest ensuring it&#8217;s set to Any time.

**Step 2:** Select the messages you want to export.  This is where the UI takes a bit of getting used to.  First, select any single text message from the hamburger menu in the upper left.

<img class="alignnone size-medium wp-image-1418" src="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2017/11/01.png?resize=169%2C300&#038;ssl=1" alt="" width="169" height="300" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2017/11/01.png?resize=169%2C300&ssl=1 169w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2017/11/01.png?w=576&ssl=1 576w" sizes="(max-width: 169px) 100vw, 169px" data-recalc-dims="1" /> 

<img class="alignnone size-medium wp-image-1419" src="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2017/11/02.png?resize=169%2C300&#038;ssl=1" alt="" width="169" height="300" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2017/11/02.png?resize=169%2C300&ssl=1 169w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2017/11/02.png?w=576&ssl=1 576w" sizes="(max-width: 169px) 100vw, 169px" data-recalc-dims="1" /> 

Then you will have the option to multi-select messages.  Click on that and then click on the hamburger menu again.  Now you have the option to multi-select entire message strings.

<img class="alignnone size-medium wp-image-1420" src="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2017/11/03.png?resize=169%2C300&#038;ssl=1" alt="" width="169" height="300" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2017/11/03.png?resize=169%2C300&ssl=1 169w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2017/11/03.png?w=576&ssl=1 576w" sizes="(max-width: 169px) 100vw, 169px" data-recalc-dims="1" /> 

NOTE: You will not be able to import group SMS/Text messages so don&#8217;t bother exporting them right now.  Just skip them, still working on that part.

Step 3: Click the save button (you may need to do it twice).  Here you will have the option to save into an Android XML format.  That is the one you want to select.

<img class="alignnone size-medium wp-image-1421" src="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2017/11/04.png?resize=169%2C300&#038;ssl=1" alt="" width="169" height="300" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2017/11/04.png?resize=169%2C300&ssl=1 169w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2017/11/04.png?w=576&ssl=1 576w" sizes="(max-width: 169px) 100vw, 169px" data-recalc-dims="1" /> 

NOTE: The app limits your export to 100 messages by default.  For $0.99 you can increase it to 1000 message.  For $5.99 you can increase it to unlimited.  Figured I would give the Windows Phone developer one last paycheck from me.

Step 4: When asked where you want to save these files.  Save them to OneDrive somewhere.

Step 5: After the file has sync&#8217;ed to OneDrive, open up the Android OneDrive application.  This is critical.  You MUST download the file from OneDrive and make it available offline.

Step 6: Go to the Android Store and download <a href="https://play.google.com/store/apps/details?id=com.riteshsahu.SMSBackupRestore" target="_blank" rel="noopener">SMS Backup and Restore</a> from Carbonite.

Step 7: Go back to OneDrive on your Android device, click on the .xml files and it should ask if you want to restore the SMS Messages.  Click yes.

NOTE: The app will tell you this but SMS Backup and Restore has to become the default SMS app for this to work.  It walks you through the entire process and even tells you after the fact to put it back.  But another warning is good.

Done.  Open up messages and you should now have your old text messages from your Windows 10 Mobile device.