---
id: 203
title: Backup for Lync Server 2013
date: 2013-03-31T06:04:46+00:00
author: Richard Brynteson
layout: post
guid: http://masteringlync.com/?p=203
permalink: /2013/03/31/backup-for-lync-server-2013/
st_layout_box:
  - right
categories:
  - Lync Server 2013
---
Often I get asked what should someone backup for Lync Server 2013 and thankfully the list of items hasn&#8217;t changed that much from Lync Server 2010 to Lync Server 2013.  Back in [June of 2012](http://masteringlync.com/2012/06/26/investigating-backup-and-disaster-recovery-in-lync-2010/) I wrote an article on how to backup and recovery Lync Server 2010 and I highlighted a few major items:

  * CMS Database
  * LIS Database
  * Response Groups
  * User Data

Things haven&#8217;t changed that much and with the exception of maybe adding your CDR/Archiving and Persistent Chat data to this list but otherwise it is still a very complete list.  With Lync Server 2013 we get a significantly better pool pairing process that does much of this replication of data from point A to point B but as any network administrator is aware, you should ALWAYS have a backup plan that involves several different ways to recover data.

So below is a script I have created for my Lync 2013 Deployments.  I know there are a ton of different scripts out on the web for backup of Lync but I typically have two issues.  First, I&#8217;m not a PowerShell expert and many times when I look at the scripts I can&#8217;t seem to figure out what is going on.  Second, backing up data is only one step of the puzzle, the second item is to get that data somewhere else.  So I always make sure my scripts have a robocopy process to get information from one side to the other and with multiple versions of it.

This script has been tested with both Standard and Enterprise Edition pools and on Windows Server 2012.

_\# Export Script for Backup_  
_\# Created By Richard Brynteson_  
_\# Avtex 2013_

_\# Import Lync Module_  
_Import-Module &#8220;C:Program FilesCommon FilesMicrosoft Lync Server 2013ModulesLyncLync.psd1&#8221;_

_###Variables To Set_  
_$folderPath = &#8220;D:BackupFromMSP&#8221;_  
_$lengthOfBackup = &#8220;-10&#8221;_  
_$drFolderPath = &#8220;[\drserver.domain.comFromMSP$](//\blmvmlync01.corp.avtex.comFromMSP$)&#8221;_  
_$poolFQDN = &#8220;pool.domain.com&#8221;_  
_###Done_

_#Production &#8211; Delete Older Than x Days_  
_get-childitem $folderPath -recurse | where {$\_.lastwritetime -lt (get-date).adddays($lengthOfBackup) -and -not $\_.psiscontainer} |% {remove-item $_.fullname -force }_

_#Production &#8211; Delete Empty Folders_  
_$a = Get-ChildItem $folderPath -recurse | Where-Object {$_.PSIsContainer -eq $True}_  
_$a | Where-Object {$_.GetFiles().Count -eq 0} | Remove-Item_

_#Production &#8211; Get Date and Create Folder_  
_$currDate = get-date -uformat “%a-%m-%d-%Y-%H-%M”_  
_New-Item $folderPath$currDate -Type Directory_

_#Delete Older Than x Days – DR Side_  
_get-childitem $drFolderPath -recurse | where {$\_.lastwritetime -lt (get-date).adddays($lengthOfBackup) -and -not $\_.psiscontainer} |% {remove-item $_.fullname -force }_

_#Delete Empty Folders – DR Side_  
_$a = Get-ChildItem $drFolderPath -recurse | Where-Object {$_.PSIsContainer -eq $True}_  
_$a | Where-Object {$_.GetFiles().Count -eq 0} | Remove-Item_

_#Message Out_  
_Write-Host -ForegroundColor Green “Backup to server in progress”_

_#Export CMS/XDS and LIS_  
_Export-CsConfiguration -FileName $folderPath$currDateXdsConfig.zip_  
_Export-CsLisConfiguration -FileName $folderPath$currDateLisConfig.zip_

_#Export Voice Information_  
 _Get-CsDialPlan | Export-Clixml -path $folderPath$currDateDialPlan.xml_  
 _Get-CsVoicePolicy | Export-Clixml -path $folderPath$currDateVoicePolicy.xml_  
 _Get-CsVoiceRoute | Export-Clixml -path $folderPath$currDateVoiceRoute.xml_  
 _Get-CsPstnUsage | Export-Clixml -path $folderPath$currDatePSTNUsage.xml_  
 _Get-CsVoiceConfiguration | Export-Clixml -path $folderPath$currDateVoiceConfiguration.xml_  
 _Get-CsTrunkConfiguration | Export-Clixml -path $folderPath$currDateTrunkConfiguration.xml_

_#Export RGS Config_  
_Export-CsRgsConfiguration -Source &#8220;service:ApplicationServer:$poolFQDN&#8221; -FileName $folderPath$currDateRgsConfig.zip_

_#Export User Information_  
_Export-CsUserData -PoolFqdn $poolFQDN -FileName $folderPath$currDateUserData.zip_

_Write-Host -ForegroundColor Green “XDS, LIS, User and RGS backup to server is completed.  Files are located at $folderPath$currDate”_  
_Write-Host -ForegroundColor Green “Please make sure to export Voice Configuration”  _ 

_#Copy Files to DR Server_  
_robocopy $folderPath $drFolderPath /COPY:DATSO /S_

**How to Use The Script**

I have made it as simple as I can to use and reuse.  First off, save the above to a file names LyncBackupScript.ps1 and save it to D:backup.  In terms of variables, I have four of them.  The first one is the location of where the backup files are actually going to be stored.  So let&#8217;s say this script is at D:Backup then I&#8217;m going to store my files in D:BackupFromMSP.  Within that folder, the script is going to create a folder for each day.  The length of backup is how many files are we going to keep on the server.  The number must be a negative number, like -10 days.  DR folder path works like above.  The Pool is the name of the pool you want to backup.

You will want to schedule this script as a scheduled task.  So your scheduled task would be something like:

powershell.exe -FileName d:backuplyncbackupscript.ps1

So go to Administrative Tools | Scheduled Tasks.  Create a new task (Lync Backup for Example).  The path to the executable is above and set it to be scheduled on a daily basis.  After you have walked through the wizard and picked a particular day, make sure to edit the backup script.  You will want to change the setting to allow the script to run anytime.  When you do that, you will need to make whatever user is going to run the script with Logon as Batch permissions.  You can do this through the Local Security Policy.  Lastly, in terms of permissions, you will need to be a member of the RTCUniversalServerAdmins group to execute this.

I&#8217;ll add some pretty screen shots to this over the coming days but it&#8217;s late.  Next to add is a little simple error handling and a HTML E-Mail Report on failures/success.  But that is a project for another night/time.

UPDATE &#8211; I have updated the script with the provided info on how to backup voice configuration items.

&nbsp;