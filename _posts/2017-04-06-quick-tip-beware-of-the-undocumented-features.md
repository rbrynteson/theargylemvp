---
id: 1375
title: 'Quick Tip: Beware of the Undocumented Features'
date: 2017-04-06T16:29:43+00:00
author: Richard Brynteson
layout: post
guid: http://masteringlync.com/?p=1375
permalink: /2017/04/06/quick-tip-beware-of-the-undocumented-features/
panels_data:
  - 'a:0:{}'
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
categories:
  - Lync Server 2013
  - Skype for Business
---
Sometimes you are just blown away by the simplicity of a problem.  I&#8217;m going to let pictures do all of the talking on this.  All you need to know is environment is Polycom VVX Phones.

**Before**

feature.presence.enabled=&#8221;0&#8243;

<a href="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2017/04/Before1.png" rel="attachment wp-att-1376"><img class="wp-image-1376 alignnone" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2017/04/Before1.png?resize=600%2C580&#038;ssl=1" alt="Before1" width="600" height="580" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2017/04/Before1.png?w=1550&ssl=1 1550w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2017/04/Before1.png?resize=300%2C290&ssl=1 300w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2017/04/Before1.png?resize=768%2C742&ssl=1 768w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2017/04/Before1.png?resize=1024%2C990&ssl=1 1024w" sizes="(max-width: 600px) 100vw, 600px" data-recalc-dims="1" /></a>

Syslog from VVX Phone.

<a href="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2017/04/Before2.png" rel="attachment wp-att-1377"><img class="wp-image-1377 alignnone" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2017/04/Before2.png?resize=600%2C406&#038;ssl=1" alt="Before2" width="600" height="406" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2017/04/Before2.png?w=1402&ssl=1 1402w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2017/04/Before2.png?resize=300%2C203&ssl=1 300w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2017/04/Before2.png?resize=768%2C519&ssl=1 768w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2017/04/Before2.png?resize=1024%2C692&ssl=1 1024w" sizes="(max-width: 600px) 100vw, 600px" data-recalc-dims="1" /></a>

Data measured in Bytes

**After**

feature.presence.enabled=&#8221;1&#8243;

<a href="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2017/04/After1.png" rel="attachment wp-att-1378"><img class="alignnone wp-image-1378" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2017/04/After1.png?resize=600%2C517&#038;ssl=1" alt="After1" width="600" height="517" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2017/04/After1.png?w=1202&ssl=1 1202w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2017/04/After1.png?resize=300%2C258&ssl=1 300w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2017/04/After1.png?resize=768%2C661&ssl=1 768w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2017/04/After1.png?resize=1024%2C882&ssl=1 1024w" sizes="(max-width: 600px) 100vw, 600px" data-recalc-dims="1" /></a>

Phones set to pickup changes between Midnight and 5 AM.

Don&#8217;t worry&#8230;I&#8217;ll update this post with more details but the pictures are amazing.  The key item to know is that feature.presence.enabled=&#8221;0&#8243; is apparently not supported in a Skype for Business environment.  We have contacted Polycom for a list of all other settings that can be modified while Skype for Business profile is selected but are not documented.