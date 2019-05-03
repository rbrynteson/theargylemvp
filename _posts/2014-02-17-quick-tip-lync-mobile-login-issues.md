---
id: 651
title: 'Quick Tip: Lync Mobile Login Issues'
date: 2014-02-17T17:52:57+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=651
permalink: /2014/02/17/quick-tip-lync-mobile-login-issues/
st_layout_box:
  - right
panels_data:
  - 'a:0:{}'
categories:
  - Lync Server 2013
---
**Scenario**: You have verified that everything is working and setup correctly.  You have confirmed this using the <a href="http://technet.microsoft.com/en-us/library/jj907302.aspx" target="_blank">Lync Connectivity Analyzer</a> but you are having login issues.  This may even manifest itself where Windows Phone and Android Lync 2013 Mobile client can login without any issues but iOS devices are unable to login.

**Issue**: Looking into the logs of the iOS device that isn&#8217;t able to login, we see the following information in the log:

Search for _INFO TRANSPORT CMetaDataRequest.cpp/90:MEX response received_.

Below that section, you should see POST <https://lws.domain.com/webticket/webticketservice.svc>

And then right below that we get the following:

<p style="padding-left: 30px">
  <!DOCTYPE html PUBLIC &#8220;-//W3C//DTD XHTML 1.0 Strict//EN&#8221; &#8220;<a href="http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd</a>&#8220;><br /> <html xmlns=&#8221;<a href="http://www.w3.org/1999/xhtml">http://www.w3.org/1999/xhtml</a>&#8220;><br /> <head><br /> <meta http-equiv=&#8221;Content-Type&#8221; content=&#8221;text/html; charset=iso-8859-1&#8243;/><br /> <title>401 &#8211; Unauthorized: Access is denied due to invalid credentials.</title><br /> <style type=&#8221;text/css&#8221;>
</p>

If you open your browser and go to <https://lws.domain.com/webticket/webticketservice.svc> you receive a 401 unauthorized.  This is not the behavior you should expect.  You should be getting prompted for credentials.

**Solution**: On all of your front-end servers, launch the IIS Manager.  Expand out Lync Server External Web Services and find WebTicket.  Double click on Authentication.

[<img class="alignnone wp-image-652 size-medium" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/02/pic12-296x300.png?resize=296%2C300&#038;ssl=1" alt="pic1" width="296" height="300" srcset="https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/02/pic12.png?resize=296%2C300&ssl=1 296w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/02/pic12.png?resize=100%2C100&ssl=1 100w, https://i0.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/02/pic12.png?w=404&ssl=1 404w" sizes="(max-width: 296px) 100vw, 296px" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.com/files/2014/02/pic12.png)

Here you will see the options enabled for WebTicket.  Yours should look like this:

[<img class="alignnone wp-image-653 size-full" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2014/02/pic21.png?resize=474%2C176&#038;ssl=1" alt="pic2" width="474" height="176" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/02/pic21.png?w=474&ssl=1 474w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2014/02/pic21.png?resize=300%2C111&ssl=1 300w" sizes="(max-width: 474px) 100vw, 474px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.com/files/2014/02/pic21.png)

I found that in our deployment, Windows Authentication was disabled.  Once this was Enabled again, iOS devices immediately were able to login again.

What I don&#8217;t know is how this got turned off but it&#8217;s definitely supposed to be turned on.

&nbsp;