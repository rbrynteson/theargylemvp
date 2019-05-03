---
id: 1668
title: The Curious Case of TCP/9808
date: 2018-09-23T16:30:18+00:00
author: Richard Brynteson
layout: post
guid: http://masteringlync.com/?p=1668
permalink: /2018/09/23/the-curious-case-of-tcp9808/
colormag_page_layout:
  - default_layout
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
image: https://masteringlync.com/wp-content/uploads/sites/2/2018/09/mystery-logo.jpg
categories:
  - Skype for Business
---
Sometimes a mystery is so satisfying once you have solved it.  Check out this amazing story from co-worker and amazing guy Mitch Steiner.

### Prologue

The Scene: A typical day in Ops  
Ops engineer: Oh, I just got an alert that the default certificate on one of our enterprise pools is about to expire. Time to create a change request for the upcoming maintenance window and get it replaced.

### Act 1 Scene 1

(Two days later , during the weekly maintenance window)  
Our trusty ops engineer , having filed the proper change request and obtaining an approval, creates a CSR , submits it to his internal PKI and  generates  a new default certificate. He launches the deployment wizard,  imports his certificate ,  and assigns it to the proper usages ( server default , internal and external web services).  
No errors were found, and all appears  working as expected.  
Following his organization&#8217;s pre defined best practices (trust but verify!)  ,  our ops engineer now runs through his certificate replacement checklist.  
He grabs his trusty [DigiCert utility for windows](https://www.digicert.com/util/) , and proceeds to verify that the new certificate is being presented on the known ports  ( 5061, 443, 4443). To accomplish this, he launches the tool on the enterprise pool server , selects tools and  clicks “check install” in the  certificate installation checker section. He sets his server address  to localhost , sets  the SSL mode to  direct and checks each port , verifying the new “valid to”  date and serial number match the newly provisioned certificate  (Exhibit A is shown below)

<a href="http://blog.thepbxisdead.com/2018/09/the-curious-case-of-tcp9808.html?spref=tw" target="_blank" rel="noopener">Continue Reading</a>&#8230;