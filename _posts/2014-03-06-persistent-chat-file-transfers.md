---
id: 665
title: Understanding Persistent Chat File Transfers
date: 2014-03-06T03:21:03+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=665
permalink: /2014/03/06/persistent-chat-file-transfers/
st_layout_box:
  - right
panels_data:
  - 'a:0:{}'
categories:
  - Lync Server 2013
---
One of the new features of [Lync 2013 February 2014](http://blogs.technet.com/b/lync/archive/2014/02/26/february-2014-update-for-the-lync-desktop-client.aspx) (CU5/SP1) update that has gone under the radar is the ability to do file transfers from Lync to persistent chat room.  To me this is a BIG add to the client because it&#8217;s been one of the deployment blockers of Persistent Chat.  Everyone loves the idea of persistent chat but they want to be able to use all of the features in a single client and file transfer was a major miss at RTM.

**What does it look like?**

If you are familiar with file transfer in Lync 2013 client, than you know exactly what persistent chat looks like.  Here is a quick screen shot.

[<img class="alignnone wp-image-667 size-medium" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/03/pic3-300x205.png?resize=300%2C205&#038;ssl=1" alt="pic3" width="300" height="205" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/03/pic3.png?resize=300%2C205&ssl=1 300w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/03/pic3.png?w=491&ssl=1 491w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.com/files/2014/03/pic3.png)

From here you can open or save the file.

**How does it work?**

We can start by looking at a quick diagram of the traffic flow.

[<img class="alignnone wp-image-666 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/03/pic2.png?resize=341%2C311&#038;ssl=1" alt="pic2" width="341" height="311" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/03/pic2.png?w=341&ssl=1 341w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/03/pic2.png?resize=300%2C274&ssl=1 300w" sizes="(max-width: 341px) 100vw, 341px" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.com/files/2014/03/pic2.png)

Here we can see there are two protocols in play.  XCCOS over SIP is the protocol that Group Chat (formerly Parlano) was built on top of.  With Lync, we take that messaging protocol and wrap it in SIP.  The second is HTTPS, which is the real work horse in this scenario.

When you copy a file into the Lync client it appears the file is being upload via HTTPS.  Here is what we find in Fiddler:

[<img class="alignnone wp-image-668 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/03/pic4.png?resize=585%2C87&#038;ssl=1" alt="pic4" width="585" height="87" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/03/pic4.png?w=585&ssl=1 585w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/03/pic4.png?resize=300%2C45&ssl=1 300w" sizes="(max-width: 585px) 100vw, 585px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.com/files/2014/03/pic4.png)

We can see four SOAP requests to the front-end server that hosts the Persistent Chat end-point.  You can run a get-cspersistentchatendpoint to get back detailed information our PChat object:

[<img class="alignnone wp-image-669 size-full" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/03/pic5.png?resize=646%2C151&#038;ssl=1" alt="pic5" width="646" height="151" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/03/pic5.png?w=646&ssl=1 646w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/03/pic5.png?resize=300%2C70&ssl=1 300w" sizes="(max-width: 646px) 100vw, 646px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.com/files/2014/03/pic5.png)

**NOTE**: It&#8217;s important to remember that all users, contacts, objects follow pool pairing.  So if lyncfe03 was in a pool pairing relationship with another front-end pool and that pool was failed over, than the persistent chat endpoint would register with the backup pool.

If we dig into each of the four soap requests we have:<figure id="attachment_670" style="width: 578px" class="wp-caption alignnone">

[<img class="wp-image-670 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/03/pic6.png?resize=578%2C96&#038;ssl=1" alt="pic6" width="578" height="96" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/03/pic6.png?w=578&ssl=1 578w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/03/pic6.png?resize=300%2C50&ssl=1 300w" sizes="(max-width: 578px) 100vw, 578px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.com/files/2014/03/pic6.png)<figcaption class="wp-caption-text">Get Unique Name for File Extension</figcaption></figure> <figure id="attachment_671" style="width: 568px" class="wp-caption alignnone">[<img class="wp-image-671 size-full" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/03/pic7.png?resize=568%2C65&#038;ssl=1" alt="pic7" width="568" height="65" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/03/pic7.png?w=568&ssl=1 568w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/03/pic7.png?resize=300%2C34&ssl=1 300w" sizes="(max-width: 568px) 100vw, 568px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.com/files/2014/03/pic7.png)<figcaption class="wp-caption-text">Get Max File Upload Allowed</figcaption></figure> <figure id="attachment_672" style="width: 568px" class="wp-caption alignnone">[<img class="wp-image-672 size-full" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/03/pic8.png?resize=568%2C75&#038;ssl=1" alt="pic8" width="568" height="75" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/03/pic8.png?w=568&ssl=1 568w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/03/pic8.png?resize=300%2C40&ssl=1 300w" sizes="(max-width: 568px) 100vw, 568px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.com/files/2014/03/pic8.png)<figcaption class="wp-caption-text">Get Content Size</figcaption></figure> <figure id="attachment_673" style="width: 569px" class="wp-caption alignnone">[<img class="wp-image-673 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/03/pic9.png?resize=569%2C65&#038;ssl=1" alt="pic9" width="569" height="65" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/03/pic9.png?w=569&ssl=1 569w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/03/pic9.png?resize=300%2C34&ssl=1 300w" sizes="(max-width: 569px) 100vw, 569px" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.com/files/2014/03/pic9.png)<figcaption class="wp-caption-text">Upload</figcaption></figure> 

Now that our file has been uploaded we can then go to the file share and see these files are uploaded.

[<img class="alignnone wp-image-674 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/03/pic10.png?resize=692%2C179&#038;ssl=1" alt="pic10" width="692" height="179" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/03/pic10.png?w=692&ssl=1 692w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/03/pic10.png?resize=300%2C78&ssl=1 300w" sizes="(max-width: 692px) 100vw, 692px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.com/files/2014/03/pic10.png)

**NOTE**: The files in this file share are NOT encrypted.  You can open any of these documents with whatever associated application.  For example, these are Word documents and I was able to open them up directly.  The following groups have permission to this folder: RTC Component Local group, RTCComponentUniversalServices and RTCUniversalServerAdmins.

**File Download**

This process is clearly done via HTTPS.  When saving a file we see:

[<img class="alignnone wp-image-675 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/03/pic11.png?resize=583%2C119&#038;ssl=1" alt="pic11" width="583" height="119" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/03/pic11.png?w=583&ssl=1 583w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/03/pic11.png?resize=300%2C61&ssl=1 300w" sizes="(max-width: 583px) 100vw, 583px" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.com/files/2014/03/pic11.png)

And when we look at the contents of each one we can see that it&#8217;s simply downloading the file contents:

[<img class="alignnone wp-image-676 size-full" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/03/pic12.png?resize=568%2C203&#038;ssl=1" alt="pic12" width="568" height="203" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/03/pic12.png?w=568&ssl=1 568w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/03/pic12.png?resize=300%2C107&ssl=1 300w" sizes="(max-width: 568px) 100vw, 568px" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.com/files/2014/03/pic12.png)

Hopefully this sheds a little light on the file transfer process with Persistent Chat.

**Easter Egg Feature**

According to the Lync Team Blog post about the client updates it notes that file transfer feature is only supported with internal clients.  However, as you can see, if everything is happening via HTTPS than there is no reason why this has to be true.  In my testing, I found that if you published your URL via your reverse proxy, this feature works fine externally.  I tested this with both TMG and IIS ARR as my reverse proxy.  Of course, limitation might exist if your pools are named with internal domains which cannot be published externally but it appears as though the feature exists.  My hope is that in the future they modify the process to just ride on top of the already existing web services URLs.