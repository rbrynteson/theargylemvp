---
id: 193
title: Migrating Userdata from OCS 2007/R2/Lync 2010 to Lync 2013
date: 2013-03-28T18:50:23+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=193
permalink: /2013/03/28/migrating-userdata-from-ocs-2007r2lync-2010-to-lync-2013/
st_layout_box:
  - right
categories:
  - Lync Server 2013
---
Before Lync Server 2013 when you wanted to deal with user contact list manipulation you needed to use the dbimpexp.exe tool as part of the resource kit to export your user data and import it.  This tool had some limitations and wasn&#8217;t supported for migrations between environments where the SIP name remained the same.  In Lync Server 2013 all of this functionality is now built in with the Export-CsUserData, Import-CsUserData and Update-CsUserData.

Now with these commands comes lots of new flexibility when it comes to importing and exporting data.  Additionally, the Convert-CsUserData can be used specifically to update an OCS 2007/R2 and Lync 2010 dbimpexp file and convert it into a user .zip file that can be imported into Lync Server 2013.

So there is one item I want to draw your attention to as it appears to be a great distractor in terms of using this tool.  According to the TechNet article for the Import-CsUserData there is a -LegacyFormat switch that you would think would take a legacy formatted contact.xml and import it for you &#8211; well it won&#8217;t.  Not sure if it&#8217;s a bug or not written but it will result in an error stating that it cannot find the Convert-CsLegacyUserData cmdlet.  I&#8217;ll update if I hear why it&#8217;s looking for this command but it&#8217;s broken so don&#8217;t try it.

So what are the steps:

1. Convert your data from the legacy format to the Lync 2013 Format.  You can do that by using the following:

PS D:> Convert-CsUserData -InputFile .contactsv2.xml -OutputFile .contactsv2.zip -TargetVersion current

The TargetVersion of current means to convert it to Lync 2013.  One interesting item of note is that this command will search your Lync environment for users and verify that the SIP names exist.  So if you are using this as part of a migration you need to make sure all of the users exist in your new environment.

2. Import your data to Lync 2013

There are two commands that will get you there.  The Import-CsUserData and Update-CsUserData and although they both will get you to your final result they do very different things.  The Import-CsUserData imports your data into the back-end database which is great but its important to note that your front-end servers only reads this back-end database on cold-start.  So you would need to restart your front-end services to populate the database.  The Update-CsUserData on the other hand updates data for users who are currently logged into the system and doesn&#8217;t require a services restart.

I do want to throw a big thank you to Michael LaMontagne from Iluminari Tech for pointing me in the right direction for this task.

&nbsp;