---
id: 19
title: Lync + RemoteFX + Polycom CX600 Phone
date: 2011-10-28T21:09:46+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=19
permalink: /2011/10/28/lync-remotefx-polycom-cx600-phone/
st_layout_box:
  - right
categories:
  - Lync Server 2010
---
With Windows Server 2008 R2 + Windows 7 SP1 gives administrators all sorts of new opportunities to deploy virtualized workstations with a thin client.  One of the things we have always had to try to figure out is what can do with USB devices.  Although RemoteFX has some serious requirements on both the client and server hosting the VM side, the opportunities can be well worth it.

Recently we had a client come to us wanting to deploy Lync Server 2010 but well over 50% of their work force is thin client only users.  So there are some options if you don’t have the specs for RemoteFX like installing the Lync client locally on the thin client, but with those options come draw backs like no office integration or confusion of having the Lync client installed on both the thin client and the remote virtual machine.

So that is where RemoteFX comes into play.  With RemoteFX you get to redirect USB devices from the thin client to the virtual machine as if it is natively plugged in.  In order to do this we need both a Windows 7 SP1 thin client and a Windows 7 SP1 virtual machine in HyperV 2008 R2.  When you have everything enabled you get this:

&nbsp;

Here you can see a CX700 device being used.  Now when I launch my Lync client I get prompted to login through the USB connection, as I would expect.  When a call comes in, I see the toaster pop and can click-to-call.  However, there was one strange issue that happens out of the box.  When the call comes in, we get the toaster pop but if I answer the call by picking up the handset, the Lync Conversation window disappears.  The call is still up on the CX600/700 but I have no ability to control the call window.

[<img title="6087.clip_image002_3E69A606" src="https://i0.wp.com/blog.avtex.com/wp-content/uploads/2011/10/6087.clip_image002_3E69A606.png?resize=421%2C441&#038;ssl=1" alt="" width="421" height="441" data-recalc-dims="1" />](https://i0.wp.com/blog.avtex.com/wp-content/uploads/2011/10/6087.clip_image002_3E69A606.png)

So a little digging into the problem.  We start by getting the Configuration Information of Lync (hold CTRL + right-click on Lync client in tray) and I found that the pairing state was in error:

Unable to pair device due to remote desktop connection.

Now, I know the device is actually loaded on the machine (because it asked me to login and I see the toaster pop) but apparently something isn’t quite right.  After doing some hunting around, I found a random [blog](http://blogs.msdn.com/b/rds/archive/2010/06/10/introducing-microsoft-remotefx-usb-redirection-part-3.aspx) article (the article is actually about RemoteFX and nothing about Lync – note it even refers to Lync as Wave14) from back in November of 2010 that references a random registry key:

[HKEY\_LOCAL\_MACHINESOFTWAREPoliciesMicrosoftCommunicator] “EnableTJOCPairingRemoted”=dword:00000001

Now you might be asking, what does this key exactly do?  That is a great question.  A call over to Microsoft confirmed that this key, although unpublished, that allows the Lync client to function in a remote machine.  I triple checked the Client virtualization guide and it’s not anywhere to be found.  This key must be loaded on the virtual machine and Lync must be exited and restarted for it to take affect.  But once the key is in place, it successfully works.

Lastly, if you don’t have the specs for RemoteFX, there are several third party little apps that exist that will do USB redirection over the network.  Here is one of them for example:

<http://www.usb-over-network.com/usb-for-remote-desktop.html>

Which works with Windows XP, Vista and Windows 7.  We have successfully tested this application (I’m not suggesting this app as a solution – it was cheap and had a free demo that we could try out) on a Wyse Windows XP Embedded thin client to a Windows 7 SP1 back-end virtual machine and it worked exactly as expected – once our magical registry key was set.