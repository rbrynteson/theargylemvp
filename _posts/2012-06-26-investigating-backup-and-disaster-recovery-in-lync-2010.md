---
id: 37
title: Investigating Backup and Disaster Recovery in Lync 2010
date: 2012-06-26T14:54:03+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=37
permalink: /2012/06/26/investigating-backup-and-disaster-recovery-in-lync-2010/
st_layout_box:
  - right
categories:
  - Lync Server 2010
---
Understanding how to properly backup and maintain your Lync environment is absolutely key to success in the event of a DR scenario.  Often times not enough time is spent verifying and testing your DR plan.  So this document is an attempt to document the backup procedure and recovery needed for the purposes of DR.  There are a few good documents out there in terms of backup.  First is the TechNet article on backing up Lync and second is a great and very complete script for backup written by Traci Herr.  I&#8217;ll be offering the script I use below as well.

<http://technet.microsoft.com/en-us/library/hh202160.aspx>

[Lync Backup Instructions](http://blogs.technet.com/b/uc_mess/archive/2011/03/17/lync_2d00_server_2d00_2010_2d00_backup_2d00_instructions.aspx) &#8211; Traci Herr

**What to Backup**

In order to complete your DR testing and real-life event, it&#8217;s important that you have some critical items.

  * Backup of CS Configuration (export-csconfiguration)
  * Backup of LIS Configuration (export-cslisconfiguration)
  * Backup of user contacts (dbimpexp.exe)
  * Backup of meeting content

Other items that are handy to have are:

  * Backup of Response Group setup/information.
  * Exchange UM/AA/SA Object setup

One note here &#8211; the backup of LIS Configuration is critical!  The other items above can be gotten from other servers or you could live without contacts, but without a backup of the LIS Database, there is NO WAY to move your CMS database.  Without being able to move it, you are literally going back to the drawing board.

So what might a script look like:

<div id="divBdy">
  <div>
    <div>
      <div dir="ltr">
        <div>
          # Export Script for Backup<br /> # Created By Richard Brynteson<br /> # Avtex 2012
        </div>
        
        <div>
          # Import Lync Module<br /> Import-Module &#8220;C:Program FilesCommon FilesMicrosoft Lync Server 2010ModulesLyncLync.psd1&#8221;
        </div>
        
        <div>
        </div>
        
        <div>
        </div>
        
        <div>
          ###Variables To Set<br /> $folderPath = &#8220;D:LyncBackupBackups&#8221;<br /> ###Done
        </div>
        
        <div>
        </div>
        
        <div>
          #Delete Older Than 30 Days &#8211; Production Side<br /> get-childitem &#8220;D:LyncBackupBackups&#8221; -recurse | where {$_.lastwritetime -lt (get-date).adddays(-30) -and -not $_.psiscontainer} |% {remove-item $_.fullname -force }
        </div>
        
        <div>
        </div>
        
        <div>
          #Delete Empty Folders &#8211; Production Side<br /> $a = Get-ChildItem &#8220;D:LyncBackupBackups&#8221; -recurse | Where-Object {$_.PSIsContainer -eq $True}<br /> $a | Where-Object {$_.GetFiles().Count -eq 0} | Remove-Item
        </div>
        
        <div>
        </div>
        
        <div>
          #Delete Older Than 30 Days &#8211; DR Side<br /> get-childitem &#8220;<a href="https://owa.avtex.com/OWA/redir.aspx?C=3hVdn104jEK70A-gYRA5l2ZK0HA0J88IydAnoVHtxiUqtwOiRkiGIYtgZHByF7qbsXnhWXEWg50.&URL=file%3a%2f%2f%5c%5cmsusnwdr099%5cProdbackup%24" target="_blank">\drserverProdbackup$</a>&#8221; -recurse | where {$_.lastwritetime -lt (get-date).adddays(-30) -and -not $_.psiscontainer} |% {remove-item $_.fullname -force }
        </div>
        
        <div>
        </div>
        
        <div>
          #Delete Empty Folders &#8211; DR Side<br /> $a = Get-ChildItem &#8220;<a href="https://owa.avtex.com/OWA/redir.aspx?C=3hVdn104jEK70A-gYRA5l2ZK0HA0J88IydAnoVHtxiUqtwOiRkiGIYtgZHByF7qbsXnhWXEWg50.&URL=file%3a%2f%2f%5c%5cmsusnwdr099%5cProdbackup%24" target="_blank">\drserverProdbackup$</a>&#8221; -recurse | Where-Object {$_.PSIsContainer -eq $True}<br /> $a | Where-Object {$_.GetFiles().Count -eq 0} | Remove-Item
        </div>
        
        <div>
        </div>
        
        <div>
          #Message Out<br /> Write-Host -ForegroundColor Green &#8220;Backup to server in progress&#8221;
        </div>
        
        <div>
        </div>
        
        <div>
          #Get Date and Create Folder<br /> $currDate = get-date -uformat &#8220;%a-%m-%d-%Y-%H-%M&#8221;<br /> New-Item $folderPath$currDate -Type Directory
        </div>
        
        <div>
        </div>
        
        <div>
          #Export CMS/XDS and LIS<br /> Export-CsConfiguration -FileName $folderPath$currDateXdsConfig.zip<br /> Export-CsLisConfiguration -FileName $folderPath$currDateLisConfig.zip
        </div>
        
        <div>
        </div>
        
        <div>
          #Export RGS Config<br /> Import-Module D:LyncBackupRgsImportExport.ps1<br /> Export-CsRgsConfiguration -FileName $folderPath$currDateRgsConfig.zip -Source ApplicationServer:lyncpool01.domain.com
        </div>
        
        <div>
        </div>
        
        <div>
          #Export User Information<br /> D:LyncToolsLyncBackupdbimpexp.exe /hrxmlfile:&#8221;$folderPath$currDateUserData.xml&#8221; /sqlserver:sqlserver.domain.com
        </div>
        
        <div>
        </div>
        
        <div>
          #Export Exchange Contact Information<br /> Get-CsExUmContact | Select &#8220;AutoAttendant&#8221;,&#8221;IsSubscriberAccess&#8221;,&#8221;Description&#8221;,&#8221;DisplayNumber&#8221;,&#8221;LineURI&#8221;, &#8220;DisplayName&#8221;,&#8221;ProxyAddresses&#8221;,&#8221;HomeServer&#8221;,&#8221;EnabledForFederation&#8221;,&#8221;EnabledForInternetAccess&#8221;, &#8220;PublicNetworkEnabled&#8221;,&#8221;EnterpriseVoiceEnabled&#8221;,&#8221;EnabledForRichPresence&#8221;,&#8221;SipAddress&#8221;,&#8221;Enabled&#8221;,
        </div>
        
        <div>
          &#8220;TargetRegistrarPool&#8221;,&#8221;VoicePolicy&#8221;,&#8221;MobilityPolicy&#8221;,&#8221;ConferencingPolicy&#8221;,&#8221;PresencePolicy&#8221;,
        </div>
        
        <div>
          &#8220;RegistrarPool&#8221;,&#8221;DialPlan&#8221;,&#8221;LocationPolicy&#8221;,&#8221;ClientPolicy&#8221;,&#8221;ClientVersionPolicy&#8221;,&#8221;ArchivingPolicy&#8221;,&#8221;PinPolicy&#8221;, &#8220;ExternalAccessPolicy&#8221;,&#8221;HostedVoicemailPolicy&#8221;,&#8221;HostingProvider&#8221;,&#8221;Name&#8221;,&#8221;DistinguishedName&#8221; | Export-Csv -Path $folderPath$currDateExUMContacts.csv
        </div>
        
        <div>
          Write-Host -ForegroundColor Green &#8220;XDS, LIS, User and Exchange UM Contacts backup to server is completed.  Files are located at $folderPath$currDate&#8221;<br /> Write-Host -ForegroundColor Green &#8220;Please make sure to export Voice Configuration&#8221;
        </div>
        
        <div>
        </div>
        
        <div>
          #Copy Files to DR Server<br /> robocopy &#8220;d:lyncbackupBackups&#8221; &#8220;<a href="https://owa.avtex.com/OWA/redir.aspx?C=3hVdn104jEK70A-gYRA5l2ZK0HA0J88IydAnoVHtxiUqtwOiRkiGIYtgZHByF7qbsXnhWXEWg50.&URL=file%3a%2f%2f%5c%5cmsusnwdr099%5cProdbackup%24" target="_blank">\drserverProdbackup$</a>&#8221; /COPYALL /S
        </div>
        
        <div>
        </div>
      </div>
    </div>
  </div>
</div>

So for this script to work, in your folder along with this script you would need a copy of dbimpexp.exe (found on the server in the common files directory) and a copy of the RgsImportExport (which can be found [here](http://blogs.technet.com/b/csps/archive/2011/02/09/scriptrgsrestore.aspx)).  I have tried to comment along the way in what it does, but at a high level, it creates a director for each backup, only holds onto the last thirty days, exports the needed information and copies it to the DR server.

I don&#8217;t claim to be a PowerShell expert so feel free to modify/update as you see fit.

**The DR Experience**

So now that we have a backup of everything, we need to actually test and validate the process.  This part is important, test your DR Plan.  I mean, really test it.  Turn off your servers in the primary site, force move your CMS database and force move some users.  If you don&#8217;t do this, you will never learn the process, you will never know what items are going to come up when you really need it.  And trust me, if you are a large organization that says Lync is a Tier III application with something crazy like 3 week recovery, don&#8217;t believe them.

So how do we recover.  First, I&#8217;m assuming we have another pool (Enterprise or Standard) sitting in a DR location for this exercise.  It can be in the same central site.  You should also configure your pool for automatic failover.

So to complete your testing you should make sure your script has run and you have a backup of everything on your DR server.  Go to your production servers and turn them off.  Be nice about it, no need to pull the power or anything crazy like that.

  1. Go to your DR Standard Edition server and login as a member of RTCUniversalServerAdmins group (or domain admin).
  2. Install a clean copy of the CMS Databases (xds and lis) to your DR SQL Server.**  
    Install-CSDatabase -CentralManagementDatabase -SqlServerFqdn ** **DRSQLServer.domain.com –Clean**You need to have sysadmin rights to the SQL Server as it will drop any existing xds and lis databases.  Also make sure that you are picking the right server here.
  3. Move your CMS Database.  This task can take a little bit depending on your network size.**  
    Move-CsManagementServer -ConfigurationFileName &#8220;XdsConfig.zip&#8221; -LisConfigurationFileName &#8220;LisConfig.zip&#8221; –Force -Verbose**
  4. Verify that replication occurred successfully.  This can be done with two commands.  The first will show you who is the active Master FQDN.  The second shows the replication status.**  
    Get-CsManagementStoreReplicationStatus –CentralManagementStoreStatus****Get-CsManagementStoreReplicationStatus | ft ReplicaFqdn, UpToDate**
  5. Force move your conference directories.  This is important.  If you are doing testing, don&#8217;t do this step.  You are going to have to trust it works.  The command would be this:**  
    Get-CsConferenceDirectory | where {$_.ServiceID -match “UserServer:**_lyncpool01.domain.com”_ **}******| Move-CsConferenceDirectory -TargetPool  lyncpool02.domain.com _-Force_****You will also need to restore your conference directory information as well
  6. Next, we need to force move all of the users from the pool to the DR pool.  Again, if you are testing, I would suggest just picking 10 users for the purpose of the test.**  
    Get-CsUser | where {$_.registrarpool –match “**_lyncpool01.domain.com_**”} | Move-CsUser –target “_lyncpool02.domain.com_” –Force**
  7. Next, you should move all of your external DNS records.  These would include items like your meet and dialin simple URLs.
  8. Now we are going to import in contact/conferencing inforomation for users.  Again, in a real DR scenario you would want to run the first command but given we are testing, you should choose the import on an individual basis.Import All &#8211; dbimpexp.exe /import /hrxmlfile:&#8221;c:BackupUser.xml&#8221; /sqlserver:**DRSQLServer.domain.com** /restype:all  
    Import Individual Records &#8211;  
    dbimpexp.exe /import /hrxmlfile: &#8220;c:BackupUser.xml&#8221; /user:<sip URL> /sqlserver:**DRSQLServer.domain.com** /restype:user

At this point in time, users should now have their contacts back and would be running on the DR Server.  As you go through this process, you should make sure you have someone taking notes with you.  Additionally, all of this information should be typed up and I would recommend storing a copy of the document on both the production servers and DR servers.  Don&#8217;t store these directions in e-mail or some other electronic form if that far side is down.