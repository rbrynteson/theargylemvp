---
title: Migrating Users to Teams Only with MFA Admin Enabled
date: 2019-08-14T12:37:39+00:00
author: Richard Brynteson
layout: post
permalink: /2019/08/14/migrating-users-to-teams-with-mfa-admin/
categories:
  - Microsoft Teams
  - Skype for Business
---

Any good organization knows that you need to enable two-factor (MFA) on your admin accounts.  But what happens when you try to go to the SfB Control Panel or PowerShell and attemp to move those users.  Unfortunately, both SfB On-Prem PowerShell and the Control Panel are not MFA aware.  So this is where App Passwords come in.

## App Password

To setup your App Password, you need to to login [https://portal.office.com](https://portal.office.com/) and click on My Account from the upper right corner.

<img src="https://theargylemvp.com/assets/images/81401.png" />

Once you have opened up the My Account page, you need to click on Settings (the gear box) at the top of the page.  A window will fly out from the right and when you browse down you will find Create and Manage App Passwords.

<img src="https://theargylemvp.com/assets/images/81402.png" />

From here you will see all devices setup for your MFA.  Click on Add Security Info.

<img src="https://theargylemvp.com/assets/images/81403.png" />

Click on App Password

<img src="https://theargylemvp.com/assets/images/81404.png" />

Come up with some clever name.

<img src="https://theargylemvp.com/assets/images/81405.png" />

And copy the password given to you.  You will NEVER see this password again so please make sure to copy it down and store it somewhere safe!

<img src="https://theargylemvp.com/assets/images/81406.png" />

## Move The User

Now we get to the easy part.  Simply launch PowerShell and run these commands:

1. $cred=Get-Credential
2. $url="https://admin1a.online.lync.com/HostedMigration/hostedmigrationService.svc" -OverrideAdminDomain [domain].onmicrosoft.com
3. Move-CsUser -Identity username@contoso.com -Target sipfed.online.lync.com -MoveToTeams -Credential $cred -HostedMigrationOverrideUrl $url

Remember, if you have anything crazy going on with your lyncdiscover address you may want to use the OverrideAdminDomain as you see in the above example.  Otherwise you can remove it.

When you are prompted for your username and password, simply use your UPN and the App Password you created before.  You are now logged in.

Lastly, as you get ready to move your users, if the users do not already have an audio conferencing license assigned in O365 you may need to use the -BypassAudioConferencingCheck as you see in my screenshot.  I left the error so everyone could enjoy it.

<img src="https://theargylemvp.com/assets/images/81407.png" />