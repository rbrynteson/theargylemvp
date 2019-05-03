---
id: 1779
title: Update Your Phones (Soon!)
date: 2019-04-30T06:57:20+00:00
author: Richard Brynteson
layout: post
guid: http://masteringlync.com/?p=1779
permalink: /2019/04/30/update-your-phones-soon/
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
image: https://masteringlync.com/wp-content/uploads/sites/2/2017/12/4316445.jpg
categories:
  - Uncategorized
---
On Thursday of last week, Microsoft dropped a post on the Tech Communities page with pretty much no context. Here is the [announcment](https://techcommunity.microsoft.com/t5/Skype-for-Business-Blog/OAuth-2-0-and-third-party-application-ID/ba-p/482876):

&#8220;_To provide our customers with best-in-class security across our services, Microsoft is implementing the use of&nbsp;_[_Microsoft Identity Platform 2.0_](https://docs.microsoft.com/en-us/azure/active-directory/develop/v2-permissions-and-consent)_&nbsp;(an evolution of the Azure Active Directory identity service) which uses the OAuth 2.0 authorization protocol. OAuth 2.0 is a method through which a third-party app can access web-hosted resources on behalf of a user, through a third-party application ID._

**_Effective immediately_**_, Microsoft requires all IP Phone partners with Skype for Business certified IP Phones to use Azure AD tenant specific third-party application ID._

_As result of this change, Skype for Business IP Phone partners have made a code change to use partner specific application ID. When deployed, the customer tenant admin will be required to confirm consent to allow the third-party phone application to be granted the necessary permissions (the same permissions currently being used by Skype for Business IP Phones)._&#8220;

Anytime you have the words &#8211; effective immediately &#8211; in a blog post about a customers enterprise environment that will get some people&#8217;s attention. So what is happening under the hood is that each phone provider now has to use their own App ID for approval into your O365 environment. Today, Microsoft gave an internal App ID to phone vendors to embed in their 3PIP phones so they could get access to services. The users of course had to still supply username/password but it was hiding under there.

So now each vendor will have it&#8217;s own App ID. 

  * [Polycom](https://login.microsoftonline.com/common/adminconsent?client_id=a850aaae-d5a5-4e82-877c-ce54ff916282&redirect_uri=https://dialin.plcm.vc/teams/postconsent.html)
  * [Crestron](https://support.crestron.com/app/answers/answer_view/a_id/1000349)
  * AudioCodes
  * Yealink

And when you click on the link, then you approve that vendors App ID into your O365 tenant. [Tom](https://tomtalks.blog/2019/04/all-skype-for-business-ip-phones-must-be-firmware-updated-by-july-1st-2019-to-continue-to-sign-into-office-365/) has some cool screen shots of this along with a few FAQ&#8217;s on it. One thing I wanted to add to the list is a table because I think it&#8217;s easier to understand what I need to do.

<table class="wp-block-table">
  <tr>
    <td>
      Deployment Type
    </td>
    
    <td>
      User Homed
    </td>
    
    <td>
      Impact Statement
    </td>
  </tr>
  
  <tr>
    <td>
      Teams / SfB 3PIP phones using Cloud Interop Gateway
    </td>
    
    <td>
      Online
    </td>
    
    <td>
      <br />All phones must be updated by July 1st and tenant admins must have approved phone partners App ID
    </td>
  </tr>
  
  <tr>
    <td>
      SfB Online
    </td>
    
    <td>
      Online
    </td>
    
    <td>
      <br />All phones must be updated by July 1st and tenant admins must have approved phone partners App ID
    </td>
  </tr>
  
  <tr>
    <td>
      SfB On-Premises with Modern Auth Enabled
    </td>
    
    <td>
      Online or On-Premises
    </td>
    
    <td>
      <br />All phones must be updated by July 1st and tenant admins must have approved phone partners App ID
    </td>
  </tr>
  
  <tr>
    <td>
      SfB On-Premises WITHOUT Modern Auth Enabled
    </td>
    
    <td>
      Online
    </td>
    
    <td>
      <br />All phones must be updated by July 1st and tenant admins must have approved phone partners App ID
    </td>
  </tr>
  
  <tr>
    <td>
      <br />SfB On-Premises WITHOUT Modern Auth Enabled
    </td>
    
    <td>
      On-Premises
    </td>
    
    <td>
      No Impact
    </td>
  </tr>
  
  <tr>
    <td>
      SfB No Hybrid
    </td>
    
    <td>
      On-Premises
    </td>
    
    <td>
      No Impact
    </td>
  </tr>
</table>

IP Phone vendors are working hard to get firmware updated that will allow their phones to play nice with this new security model.