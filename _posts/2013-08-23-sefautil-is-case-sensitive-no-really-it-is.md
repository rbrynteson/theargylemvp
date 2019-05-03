---
id: 365
title: 'SEFAUtil is case sensitive&#8230;no really it is!'
date: 2013-08-23T17:47:30+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=365
permalink: /2013/08/23/sefautil-is-case-sensitive-no-really-it-is/
st_layout_box:
  - right
categories:
  - Lync Server 2010
  - Lync Server 2013
---
So in the midst of a deployment that is heavy into delegation and they are not yet ready to deploy Enterprise Voice so controlling delegation in the Lync Client isn&#8217;t an option right now.  So that means it&#8217;s time to run off and use SEFAUtil for the task.

There are plenty of articles out there that describe how to setup SEAFUtil so I won&#8217;t cover that for now.  So what we were trying was to remove a delegate so our command was:

SEFAUtil.exe /server: lyncserver.contoso.com sip:katarina@contoso.com /removedelegate:joe@contoso.com

This would generate something like:

User Aor: sip:katarina@contoso.com  
Display Name: Katarina User  
UM Enabled: False  
Simulring enabled: False  
Simultaneously Ringing Delegates: Joe@contoso.com

So the command executed but it didn&#8217;t actually remove joe as a delegate.  We could go into the client and see that the user was indeed still a delegate.  So we tried all sorts of random items but as a last shot we decided to try to do this:

SEFAUtil.exe /server: lyncserver.contoso.com sip:katarina@contoso.com /removedelegate:Joe@contoso.com

Notice, the difference was joe@contoso.com to Joe@contoso.com &#8211; capital J.  We ran the command again and then we got:

User Aor: sip:katarina@contoso.com  
Display Name: Katarina User  
UM Enabled: False  
Simulring enabled: False  
CallForwarding Enabled: False

So the key is to look at the name that is provided in the Simultaneously Ringing Delegates section and make sure whatever the output is there matches your remove command.