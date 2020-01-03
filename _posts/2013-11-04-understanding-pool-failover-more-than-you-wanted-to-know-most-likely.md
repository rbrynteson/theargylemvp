---
id: 477
title: Understanding Pool Failover (more than you wanted to know most likely)
date: 2013-11-04T20:54:28+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=477
permalink: /2013/11/04/understanding-pool-failover-more-than-you-wanted-to-know-most-likely/
st_layout_box:
  - right
panels_data:
  - 'a:0:{}'
categories:
  - Lync Server 2013
---
In this post we will dissect the actual pool failover and failback process and what is happening under the hood when you run an invoke-cspoolfailover.  To be honest, I&#8217;m not sure what practical purpose this information serves other then I find it super interesting.

**Scenario**

In this scenario we have two front-end servers: Lyncfe03 and Lyncfe04 both Standard Edition servers.  They are setup in a pool pairing relationship.  To start with we will check the backup status on both servers.

<img class="alignnone wp-image-478 size-full" src="https://masteringlync.com/wp-content/uploads/2013/11/pic1.png?resize=768%2C192&ssl=1 768w" sizes="(max-width: 800px) 100vw, 800px" data-recalc-dims="1" />

Here we can see that both servers are in a FinalState and NormalState so we know that pool fail-over is safe to perform.

**Failing Over**

When the pool failover happens the front-end server enters an unique state that doesn&#8217;t allow client registrations.  More importantly the second pool is aware of this state and allows clients to register and get access to the information because the routing groups that were once assigned to the other pool are now loaded on the new pool.

Before we do the failover let&#8217;s check out what the registration configuration is in our environment.  You will see the importance of this in a moment.

<img class="alignnone wp-image-479 size-full" src="https://masteringlync.com/wp-content/uploads/2013/11/pic2.png?resize=300%2C129&ssl=1 300w" sizes="(max-width: 472px) 100vw, 472px" data-recalc-dims="1" />

Now that we have this basic information down we can now invoke our failover.  For this we will use this command:

<p style="padding-left: 30px">
  <em>Invoke-CsPoolFailOver -PoolFqdn lyncfe04.thelab.info -Verbose</em>
</p>

So what do we see happening in the verbose output of the failover process?  I&#8217;ll indent the output and comment along the way.

<p style="padding-left: 30px">
  <em>Get-CsRegistrarConfiguration -Identity &#8216;service:Registrar:lyncfe04.thelab.info&#8217; -LocalStore:$false</em><br /> <em> WARNING: Cannot find &#8220;RegistrarConfiguration&#8221; &#8220;Registrar:lyncfe04.thelab.info&#8221; because it does not exist.</em>
</p>

Here we search specifically for a registrar configuration for the specific pool.  None were found.

<p style="padding-left: 30px">
  <em>Backup-CsPool -PoolFqdn lyncfe04.thelab.info -LocalStore:$false -SteadyState -Category UserData</em><br /> <em> Attempting to get into steady state.</em>
</p>

Since we did not use the -DiasterMode switch when we did the failover process we do a steady state backup of UserData.

<p style="padding-left: 30px">
  <em>Invoke-CsManagementStoreReplication -ReplicaFqdn lyncfe04.thelab.info</em><br /> <em> Get-CsManagementStoreReplicationStatus -ReplicaFqdn lyncfe04.thelab.info</em>
</p>

We then ensure that the CMS is up to date.

<p style="padding-left: 30px">
  <em>Get-CsRegistrarConfiguration -Identity &#8216;service:Registrar:lyncfe04.thelab.info&#8217;</em><br /> <em> WARNING: Cannot find &#8220;RegistrarConfiguration&#8221; &#8220;Registrar:lyncfe04.thelab.info&#8221; because it does not exist.</em><br /> <em> New-CsRegistrarConfiguration -Identity &#8216;service:Registrar:lyncfe04.thelab.info&#8217; -PoolState FailingOver</em>
</p>

We check for the registrar configuration again and see it doesn&#8217;t exist.  So we create a new registrar configuration and set the PoolState to FailingOver.  This is where this gets interesting.

<p style="padding-left: 30px">
  <em>Backup-CsPool -PoolFqdn lyncfe04.thelab.info -LocalStore:$false -FullBackup -Category UserData</em><br /> <em> Attempting to get into final state.</em><br /> <em> Sync-CsUserData -PoolFqdn lyncfe04.thelab.info -Target</em><br /> <em> WARNING: Hydrating Routing Groups Owned by lyncfe04.thelab.info on Pool lyncfe03.thelab.info.</em><br /> <em> WARNING: Hydrating Routing Group {10613DBE-C944-5463-BFA3-7C1EA28D0EB9}.</em><br /> <em> WARNING: Hydrating Routing Group {8BAD09F4-4E95-58C5-BE4F-E8D7953CD0C8}.</em><br /> <em> WARNING: Hydrating Routing Group {DF7B2BD1-DD18-5B3E-A470-B3BD46B6225B}.</em>
</p>

We then do a full backup of the user data and do a sync of user data into the database.  Now since our pool (lyncfe04) is in a FailingOver state, we then see that any routing groups that were assigned to LyncFE04 get populated on LyncFE03.

<p style="padding-left: 30px">
  <em>Set-CsRegistrarConfiguration -Identity &#8216;service:Registrar:lyncfe04.thelab.info&#8217; -PoolState FailedOver</em><br /> <em> Reset-CsPoolRegistrarState -PoolFqdn lyncfe04.thelab.info -ResetType ServiceReset -Force -NoReStart</em><br /> <em> Stop-CsWindowsService -ComputerName lyncfe04.thelab.info -LeaveClsAgentRunning -NoWait -Verbose</em>
</p>

Now that our routing groups have moved over, we then change our RegistrarConfiguration from FailingOver to FailedOver.  We also reset the pool we failed from and stop services on it (leaving replication running).

Once this has completed, we can run a _Get-CsRegistrarConfiguration_ again and now we see:

<img class="alignnone wp-image-480 size-full" src="https://masteringlync.com/wp-content/uploads/2013/11/pic3.png?resize=300%2C158&ssl=1 300w" sizes="(max-width: 603px) 100vw, 603px" data-recalc-dims="1" />

As you can see LyncFE04 is now in a FailedOver state.  When this happens we can see a similar event on the LyncFE03 front-end server:

<img class="alignnone wp-image-482 size-full" src="https://masteringlync.com/wp-content/uploads/2013/11/pic4.png?resize=300%2C69&ssl=1 300w" sizes="(max-width: 497px) 100vw, 497px" data-recalc-dims="1" />

**Restart Services on 04**

So let&#8217;s go back and restart services on LyncFE04 while the registrar service is still in a FailedOver state.  When all of the services start up we see in the event viewer on LyncFE03 that LyncFE04 goes into an online state (for backup and mediation services) but from a registrar stand-point it remains in a FailedOver state.

<img class="alignnone wp-image-483 size-full" src="https://masteringlync.com/wp-content/uploads/2013/11/pic5.png?resize=768%2C78&ssl=1 768w" sizes="(max-width: 800px) 100vw, 800px" data-recalc-dims="1" />

So simply starting services is not enough to move registrar services.  This isn&#8217;t a surprise.

**Failback**

When we do the failback we see this as a series of verbose information.

<p style="padding-left: 30px">
  <em>Get-CsWindowsService -Name &#8216;RTCSRV&#8217; -ComputerName &#8216;lyncfe04.thelab.info&#8217;</em><br /> <em> Stop-CsWindowsService -LeaveClsAgentRunning -Name RTCSRV -ComputerName lyncfe04.thelab.info -NoWait -Verbose</em><br /> <em> Start-CsWindowsService -Name LYNCBACKUP -ComputerName lyncfe04.thelab.info -Verbose</em><br /> <em> Start-CsWindowsService -Name REPLICA -ComputerName lyncfe04.thelab.info -Verbose</em><br /> <em> Invoke-CsManagementStoreReplication -ReplicaFqdn lyncfe04.thelab.info</em><br /> <em> Get-CsManagementStoreReplicationStatus -ReplicaFqdn lyncfe04.thelab.info</em><br /> <em> Get-CsManagementStoreReplicationStatus -ReplicaFqdn lyncfe04.thelab.info</em><br /> <em> Get-CsManagementStoreReplicationStatus -ReplicaFqdn lyncfe04.thelab.info</em><br /> <em> Backup-CsPool -PoolFqdn lyncfe03.thelab.info -LocalStore:$false -SteadyState -Category UserData -FailedOverPoolOnly</em><br /> <em> Attempting to get into steady state.</em><br /> <em> Get-CsWindowsService -Name &#8216;RTCSRV&#8217; -ComputerName &#8216;lyncfe04.thelab.info&#8217;</em><br /> <em> Start-CsWindowsService -Name RTCSRV -ComputerName lyncfe04.thelab.info -NoWait -Verbose</em><br /> <span style="text-decoration: underline"><em> Set-CsRegistrarConfiguration -Identity &#8216;service:Registrar:lyncfe04.thelab.info&#8217; -PoolState FailingBack</em></span><br /> <em> Backup-CsPool -PoolFqdn lyncfe03.thelab.info -LocalStore:$false -FullBackup -Category UserData -FailedOverPoolOnly</em><br /> <em> Attempting to get into final state.</em><br /> <em> Sync-CsUserData -PoolFqdn lyncfe04.thelab.info</em><br /> <em> WARNING: Hydrating Routing Groups Owned by lyncfe04.thelab.info on Pool lyncfe04.thelab.info.</em><br /> <em> WARNING: Hydrating Routing Group {10613DBE-C944-5463-BFA3-7C1EA28D0EB9}.</em><br /> <em> WARNING: Hydrating Routing Group {8BAD09F4-4E95-58C5-BE4F-E8D7953CD0C8}.</em><br /> <em> WARNING: Hydrating Routing Group {DF7B2BD1-DD18-5B3E-A470-B3BD46B6225B}.</em><br /> <span style="text-decoration: underline"><em> Set-CsRegistrarConfiguration -Identity &#8216;service:Registrar:lyncfe04.thelab.info&#8217; -PoolState Active</em></span>
</p>

So I have highlighted a few items above that are worth nothing.  First you can see that we restart services on LyncFE04.  Then we set the registrar services from FailingBack and then into an Active State.  Once we have changed the PoolState we can see the routing groups rehydrate.

**Manual Change**

So now that we see what is actually happening in the background when the pool failover and failback happens we can get creative.  What happens if we just change the registrar PoolState manually?

<p style="padding-left: 30px">
  <em>Set-CsRegistrarConfiguration -Identity &#8216;service:Registrar:lyncfe04.thelab.info&#8217; -PoolState FailedOver</em>
</p>

When we run this command nothing happens immediately.  After about 90 seconds these events appear on LyncFE04:

<img class="alignnone wp-image-488 size-full" src="https://masteringlync.com/wp-content/uploads/2013/11/pic6.png?resize=300%2C97&ssl=1 300w" sizes="(max-width: 488px) 100vw, 488px" data-recalc-dims="1" />

And approximately 30 seconds after that you see this on the LyncFE03 pool:

<img class="alignnone wp-image-489 size-full" src="https://masteringlync.com/wp-content/uploads/2013/11/pic7.png?resize=300%2C52&ssl=1 300w" sizes="(max-width: 458px) 100vw, 458px" data-recalc-dims="1" />

In the background we see the client disconnect from LyncFE04 and automatically connect to LyncFE03.  Although nothing is logged on the front-end servers it appears as though the routing groups have once again been loaded on LyncFE03 again.

**Conclusion**

So what have we learned from this?

The _Set-CsRegistrarConfiguration_ can be used to determine the current status of the pool on failover and failback.  In the event of an emergency, you could potentially just &#8220;change&#8221; the settings on the registrar configuration.

The last item that is worth nothing is that the registrar configuration is created &#8220;on-the-fly&#8221; when pools are failed over and failed back.  That means if you have a setting that is special within the global configuration you should create these registrar configuration before hand otherwise the new ones created as part of the failover process will have just the default settings.