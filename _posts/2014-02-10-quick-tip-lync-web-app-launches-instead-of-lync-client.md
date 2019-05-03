---
id: 634
title: 'Quick Tip: Lync Web App launches instead of Lync Client'
date: 2014-02-10T16:51:00+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=634
permalink: /2014/02/10/quick-tip-lync-web-app-launches-instead-of-lync-client/
st_layout_box:
  - right
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
---
**Scenario**: User clicks on a Join Lync Meeting link from Outlook and the default browser launches but leaves the user within the Lync Web App, instead of launching the full Lync client.

**Issue**: Within the Windows OS there should be a file extension association for .ocsmeet that is tied back to the Lync (Desktop) or Lync (Windows Store) application.

**Resolution**: You can fix this by either reinstalling the full client or simply importing in the registry keys needed for this file extension association.  There are three places the file extension association is listed:

<span style="text-decoration: underline"><strong>Current User Software &#8211; <a href="http://masteringlync.com/wp-content/uploads/sites/2/2017/12/users_v2.txt">Download</a><br /> </strong></span>

Windows Registry Editor Version 5.00

[HKEY\_CURRENT\_USER\Software\Classes\ocsmeet\_auto\_file]

[HKEY\_CURRENT\_USER\Software\Classes\ocsmeet\_auto\_file\shell]

[HKEY\_CURRENT\_USER\Software\Classes\ocsmeet\_auto\_file\shell\edit]

[HKEY\_CURRENT\_USER\Software\Classes\ocsmeet\_auto\_file\shell\edit\command]  
@=&#8221;\&#8221;C:\\Program Files (x86)\\Microsoft Office\\Office15\\Lync.exe\&#8221; \&#8221;%1\&#8221;&#8221;

[HKEY\_CURRENT\_USER\Software\Classes\ocsmeet\_auto\_file\shell\open]

[HKEY\_CURRENT\_USER\Software\Classes\ocsmeet\_auto\_file\shell\open\command]  
@=&#8221;\&#8221;C:\\Program Files (x86)\\Microsoft Office\\Office15\\Lync.exe\&#8221; \&#8221;%1\&#8221;&#8221;

<span style="text-decoration: underline"><strong>Classes Root &#8211; <a href="http://masteringlync.com/wp-content/uploads/sites/2/2017/12/class_v2.txt">Download</a><br /> </strong></span>

Windows Registry Editor Version 5.00

[HKEY\_CLASSES\_ROOT\ocsmeet\_auto\_file]

[HKEY\_CLASSES\_ROOT\ocsmeet\_auto\_file\shell]

[HKEY\_CLASSES\_ROOT\ocsmeet\_auto\_file\shell\edit]

[HKEY\_CLASSES\_ROOT\ocsmeet\_auto\_file\shell\edit\command]  
@=&#8221;\&#8221;C:\\Program Files (x86)\\Microsoft Office\\Office15\\Lync.exe\&#8221; \&#8221;%1\&#8221;&#8221;

[HKEY\_CLASSES\_ROOT\ocsmeet\_auto\_file\shell\open]

[HKEY\_CLASSES\_ROOT\ocsmeet\_auto\_file\shell\open\command]  
@=&#8221;\&#8221;C:\\Program Files (x86)\\Microsoft Office\\Office15\\Lync.exe\&#8221; \&#8221;%1\&#8221;&#8221;

<span style="text-decoration: underline"><strong>Current User &#8211; <a href="http://masteringlync.com/wp-content/uploads/sites/2/2017/12/users_v2b.txt">Download</a><br /> </strong></span>

Windows Registry Editor Version 5.00

[HKEY\_CURRENT\_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\FileExts\.ocsmeet]

[HKEY\_CURRENT\_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\FileExts\.ocsmeet\OpenWithList]  
&#8220;MRUList&#8221;=&#8221;a&#8221;  
&#8220;a&#8221;=&#8221;Lync.exe&#8221;

[HKEY\_CURRENT\_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\FileExts\.ocsmeet\OpenWithProgids]  
&#8220;ocsmeet\_auto\_file&#8221;=hex:00

[HKEY\_CURRENT\_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\FileExts\.ocsmeet\UserChoice]  
&#8220;Progid&#8221;=&#8221;ocsmeet\_auto\_file&#8221;