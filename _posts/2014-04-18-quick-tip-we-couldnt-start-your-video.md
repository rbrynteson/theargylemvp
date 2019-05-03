---
id: 721
title: 'Quick Tip: We couldn&#8217;t start your video'
date: 2014-04-18T00:44:27+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=721
permalink: /2014/04/18/quick-tip-we-couldnt-start-your-video/
st_layout_box:
  - right
categories:
  - Lync Server 2013
---
Scenario: This issue (or at least this version of the issue) is specific to the Surface Pro 2 (all versions).  Trying to start my video I would end up with this error: &#8220;We couldn&#8217;t start your video.&#8221;

[<img class="alignnone  wp-image-722" alt="pic1" src="https://i0.wp.com/masteringlync.com/files/2014/04/pic11.png?resize=484%2C252&#038;ssl=1" width="484" height="252" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/04/pic11.png?w=691&ssl=1 691w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/04/pic11.png?resize=300%2C156&ssl=1 300w" sizes="(max-width: 484px) 100vw, 484px" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.com/files/2014/04/pic11.png)

This would happen regardless of the camera I would use, including either the built-in front or rear facing camera.  To make matters even more strange, when I would go into the Lync | Options, it would display the video preview once.  If I open up Lync | Options again, I would get an error stating the camera could not be found and restart Lync.  If I closed Lync and opened it back up, I could again show the preview once but I would never be able to start my video.

At first, I thought maybe the issue was related to my setup.  Here is a quick picture:

[<img class="alignnone  wp-image-723" alt="WP_20140415_22_01_14_Pro2" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/04/WP_20140415_22_01_14_Pro2.jpg?resize=384%2C216&#038;ssl=1" width="384" height="216" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/04/WP_20140415_22_01_14_Pro2.jpg?w=1776&ssl=1 1776w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/04/WP_20140415_22_01_14_Pro2.jpg?resize=300%2C169&ssl=1 300w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/04/WP_20140415_22_01_14_Pro2.jpg?resize=768%2C432&ssl=1 768w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/04/WP_20140415_22_01_14_Pro2.jpg?resize=1024%2C577&ssl=1 1024w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/04/WP_20140415_22_01_14_Pro2.jpg?w=1600&ssl=1 1600w" sizes="(max-width: 384px) 100vw, 384px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/04/WP_20140415_22_01_14_Pro2.jpg)

I use a Surface Pro 2 with Surface Dock to power my main screen.  I than have an [USB Pluggable](http://plugable.com/products/ud-3900) UD-3900 to power the second two monitors.  So I started by removing all of that software but nothing changed.  Removed and reinstalled Lync, back to RTM and still had the problem.

**Solution**:

A quick trip into device manager and I found that the &#8220;Surface Pro System Aggregator Firmware&#8221; had a yellow &#8220;bang&#8221; next to it.

[<img class="alignnone  wp-image-724" alt="pic2" src="https://i2.wp.com/masteringlync.com/files/2014/04/pic21.png?resize=349%2C136&#038;ssl=1" width="349" height="136" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/04/pic21.png?w=436&ssl=1 436w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/04/pic21.png?resize=300%2C117&ssl=1 300w" sizes="(max-width: 349px) 100vw, 349px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.com/files/2014/04/pic21.png)

[<img class="alignright size-full wp-image-725" alt="pic3" src="https://i2.wp.com/masteringlync.com/files/2014/04/pic31.png?resize=205%2C478&#038;ssl=1" width="205" height="478" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/04/pic31.png?w=205&ssl=1 205w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/04/pic31.png?resize=129%2C300&ssl=1 129w" sizes="(max-width: 205px) 100vw, 205px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.com/files/2014/04/pic31.png)Opening the item from here did not do anything to fix the issue.  Instead, you need to follow these steps:

1. Open up Devices and Printers.  There you will see your surface and it will have a Yellow Bang there as well.  Right-click and choose troubleshoot.

2. It will ask you to Automatically Search for a solution.  Allow the computer to grind away for a few minutes attempting to find the fix.  It won&#8217;t and it will fail miserably.

3. After a few minutes, you will get an option that tells you that you need to reboot your computer and you will have a view details options.  You need to click it!  From there, you will get a new screen like this:

[<img class="wp-image-726 alignnone" alt="pic4" src="https://i0.wp.com/masteringlync.com/files/2014/04/pic41.png?resize=362%2C377&#038;ssl=1" width="362" height="377" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/04/pic41.png?w=453&ssl=1 453w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/04/pic41.png?resize=289%2C300&ssl=1 289w" sizes="(max-width: 362px) 100vw, 362px" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.com/files/2014/04/pic41.png)

Click Restart Computer and than Lync will be fixed and your USB devices and camera will work 100%.  As I mentioned at the start, most likely an unique issue which required an unique fix but worth checking on.

&nbsp;