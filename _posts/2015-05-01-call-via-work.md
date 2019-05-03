---
id: 1133
title: Call via Work
date: 2015-05-01T15:06:24+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=1133
permalink: /2015/05/01/call-via-work/
panels_data:
  - 'a:0:{}'
categories:
  - Skype for Business
---
**Overview**

Call via Work (CvW) is the next generation and replacement for Remote Call Control (RCC).  RCC was introduced as a bridge that allowed organizations to leverage the cost of their legacy PBX system and Call via Work is being billed as the next generation of that tool.  It should be noted however, that from a functionality stand point, RCC is far more functional than Call via Work but RCC relied on additional software in the form of a SIP/CSTA server.

Call via Work on the other hand does not rely on any additional servers and utilizes the familiar and well tested Enterprise Voice functionality to &#8220;bridge&#8221; two calls together to allow organizations to continue to leverage their legacy PBX functionality.  This connectivity is done by utilizing a Direct SIP connection between Lync and the legacy PBX.

[<img class="alignnone size-full wp-image-1134" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/04/12.png?resize=509%2C490&#038;ssl=1" alt="1" width="509" height="490" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/04/12.png?w=509&ssl=1 509w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/04/12.png?resize=300%2C289&ssl=1 300w" sizes="(max-width: 509px) 100vw, 509px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/04/12.png)

EXPERT NOTE: <del>As of the writing of this document Microsoft only supports a Direct SIP connection to the PBX.  However, this is simply two Enterprise Voice calls pinned back to back on the Mediation Server and therefore using a gateway/SBC to a TDM based legacy PBX absolutely works in my testing.  Microsoft won&#8217;t support this implementation for obvious reasons but know that it will work.</del>

Update: Mr. Jamie Stark says that SBC&#8217;s are supported for Call Via Work now.  Which is good since it worked fine before.

**User Experience**

Call via Work is a solution for outbound dialing where the end-user would like to continue to use their Legacy PBX handset.  When the end user places an outbound call from their Lync client a series of events are put into motion.  First, the Lync Server makes an outbound call from the Mediation Server to the telephone number of your legacy PBX.  At this point the end users’ desk phone would start ringing.  Once the user picks up their desk phone, the Lync server makes a second call from the Mediation Server to the dialed number in the Lync Client.  This call could be to the PSTN or to another legacy PBX user.  The end user will hear the familiar outbound ringing on their desk phone.  Once the call is connected by the dialed party, the users who made the outbound call will have their presence updated to &#8220;In a Call&#8221;.  Additionally, they will have very simple call control in their Skype for Business client in the ability to hang up the call.  All other mid-call control capabilities are preserved on the legacy PBX handset.

EXPERT NOTE: The first leg of the call has absolutely no knowledge of the state of the legacy PBX station.  For example, if the user had their legacy PBX station set to forward all calls to voicemail the first leg would complete into the users voice mail and the second leg would start dialing.  The dialed user would receive the call but when they answer they would be met by the users’ voicemail greeting.

While you are using the Call via Work feature several client side features are disabled.  The call must remain a peer to peer call therefore any conferencing options have been removed from the client.

[<img class="alignnone size-full wp-image-1135" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/04/22.png?resize=540%2C524&#038;ssl=1" alt="2" width="540" height="524" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/04/22.png?w=540&ssl=1 540w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/04/22.png?resize=300%2C291&ssl=1 300w" sizes="(max-width: 540px) 100vw, 540px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/04/22.png)

If the call was an escalation from an existing IM window to another internal person, you would continue to have IM, Application and Desktop Sharing and File Transfer ability.  However, video would be blocked from the client and any escalation options to a conference would also be unavailable.

If a user attempts to make or receives a second call to their PBX Station the conversation window on the users’ workstation will not be updated to reflect this new call and will no longer be accurate.

**Inbound Calls**

The Call via Work feature is not designed for inbound calls from the PSTN however you could take steps to make this work.

**Call Flow**

The call flow (as display in the above image) is a straight forward process.  How the client initiates the call however is unlike an Enterprise Voice call from the client.

  1. The user places a call from the Skype from Business client.  It is important to realize that this feature is not available from the Lync 2013 client or when the Skype for Business is using the Lync 2013 UI.  (See the Client Changes chapter for more details on the client UI options.)[<img class="alignnone size-full wp-image-1136" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/04/31.png?resize=398%2C111&#038;ssl=1" alt="3" width="398" height="111" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/04/31.png?w=398&ssl=1 398w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/04/31.png?resize=300%2C84&ssl=1 300w" sizes="(max-width: 398px) 100vw, 398px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/04/31.png)When the user clicks the call button the Skype for Business client sends a UCWA request to the Skype for Business Front-End Server.Using Fiddler we can see the captured UCWA request being sent to the front-end server.[<img class="alignnone size-full wp-image-1137" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/04/41.png?resize=647%2C160&#038;ssl=1" alt="4" width="647" height="160" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/04/41.png?w=647&ssl=1 647w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/04/41.png?resize=300%2C74&ssl=1 300w" sizes="(max-width: 647px) 100vw, 647px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/04/41.png) 
    The responds with a 201 Created
    
    [<img class="alignnone size-full wp-image-1138" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/04/5.png?resize=362%2C224&#038;ssl=1" alt="5" width="362" height="224" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/04/5.png?w=362&ssl=1 362w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/04/5.png?resize=300%2C186&ssl=1 300w" sizes="(max-width: 362px) 100vw, 362px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/04/5.png)</li> 
    
      * Once the server has received the above UCWA request, it initiates an outbound call to the PBX.
      * The legacy PBX will route the call and ring the users PBX Station.
      * Once the call is setup to the PBX station, it starts a second call to the dialed number.
      * In this example, the PBX has routed the call to the PSTN.  But remember, this user who was called could be another PBX user.
      * The far-end user receives the call and answers.  At this point the call is established with media traveling through the Skype for Business Server acting as a back-to-back agent</ol> 
    
    **Configuration**
    
    The configuration of Call via Work requires many of the same setup and configuration of an Enterprise Voice deployment.  The steps can be broken down into three high level sections.  
    Configure Enterprise Voice  
    Take whatever steps you needed to connect your Skype for Business Pool to your legacy PBX via Direct SIP (or via Gateway).  Configuration of Enterprise Voice is beyond the scope of this chapter.  Users who want to use the Call via Work feature must be enabled for Enterprise Voice.
    
    **Call via Work Policy**
    
    Skype for Business introduces a new policy called Call via Work.  By default a global policy will be created during the installation which is disabled.
    
    [<img class="alignnone size-full wp-image-1139" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/04/6.png?resize=518%2C155&#038;ssl=1" alt="6" width="518" height="155" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/04/6.png?w=518&ssl=1 518w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/04/6.png?resize=300%2C90&ssl=1 300w" sizes="(max-width: 518px) 100vw, 518px" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/04/6.png)
    
    The Call via Work policy has three settings and are granted to users within your organization.  
    Enabled – Is the user allowed to use the Call via Work feature.  
    UseAdminCallbackNumber – Is the user required to use the defined call back number.  
    AdminCallbackNumber – What number should be called by default when making outbound calls.  
    I have to admit that the admin call back number doesn’t make a lot of sense.  If the plan is to have each user call their legacy PBX phone by first this policy makes it appear as though you would need to create a Call via Work policy for each and every user.  Take this policy as example:
    
    [<img class="alignnone size-full wp-image-1140" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/04/7.png?resize=264%2C76&#038;ssl=1" alt="7" width="264" height="76" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/04/7.png)
    
    Here we have created a Call via Work policy that is enabled and uses the default call back number of +1001.  If I were to assign this policy to multiple users they would all by default ring back to the same PBX station when making an outbound call.  This design doesn’t make a lot of sense.
    
    **Client Configuration**
    
    If you are not going to create a Call via Work policy for every user in your organization than the end-user needs to define their call handling rules from the client.  Once a user is granted a Call via Work policy the Call Forwarding section of their client becomes Call Handling.
    
    [<img class="alignnone size-full wp-image-1143" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/04/10.png?resize=715%2C588&#038;ssl=1" alt="10" width="715" height="588" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/04/10.png?w=715&ssl=1 715w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/04/10.png?resize=300%2C247&ssl=1 300w" sizes="(max-width: 715px) 100vw, 715px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/04/10.png)
    
    Here we can see that outbound calls will ring my PBX Station first before being the outbound call is made.
    
    **Inbound Calling**
    
    Call via Work wasn’t designed with inbound calling from the PSTN however steps can be taken to make this work.  That said, I don’t see many organizations taking advantage of this feature as it requires your Skype for Business Server to front-end all inbound calls – that is it must be placed upstream of your legacy PBX.  If an organization is holding onto their legacy PBX often times it’s because they have some reservations about using Skype for Business as their primary communications tool and therefore placing it upstream of the PBX will be a non-starter to those organizations.  However, for those who want this functionality here is the overall of how this would function.
    
    [<img class="alignnone size-full wp-image-1142" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/04/9.png?resize=446%2C468&#038;ssl=1" alt="9" width="446" height="468" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/04/9.png?w=446&ssl=1 446w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2015/04/9.png?resize=286%2C300&ssl=1 286w" sizes="(max-width: 446px) 100vw, 446px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2015/04/9.png)
    
      1. PSTN User makes an inbound call.
      2. The PSTN routes the call to your Skype for Business Server
      3. The Skype for Business Server routes the call to the Enterprise Voice enabled user.
      4. The user has enabled simultaneously ring to their PBX Station.
      5. The Skype for Business Server routes the call to the PBX.
      6. The PBX routes the call to the legacy PBX Station.
    
    It should be noted that the Enterprise Voice number and the PBX Station could have either the same telephone number or different numbers.  If you want to use the same telephone number in both Skype for Business and the legacy PBX you would utilize the ms-skipRnl header to allow the call to exit the Skype for Business server.
    
    In another post we will detail how to deploy in-bound Call Via Work.