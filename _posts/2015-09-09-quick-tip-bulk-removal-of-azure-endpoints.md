---
id: 1189
title: 'Quick Tip: Bulk removal of Azure Endpoints'
date: 2015-09-09T13:04:02+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=1189
permalink: /2015/09/09/quick-tip-bulk-removal-of-azure-endpoints/
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
  - Azure
---
Today we take a step outside the Skype for Business toy box and look at some PowerShell with Azure.  The goal is easy, to bulk remove all associated EndPoints with a particular VM hosted in Azure.  The most difficult part of this is simply waiting out all of the changes.  What I discovered in trying to script this is you have to invoke the Update-AzureVM after each add/removal of an EndPoint.  So if you are trying to remove or add a great number of EndPoints you should expect to wait &#8211; a long time.

$vm = Get-AzureVM -ServiceName &#8220;server01&#8221; -Name &#8220;server01&#8221;  
$endpoints = Get-AzureEndpoint -VM $vm  
foreach ($ep in $endpoints)  
{  
Remove-AzureEndpoint -VM $vm -Name ($ep.Name)  
$vm | Update-AzureVM  
}