---
title: Taking a Spin With the Teams Toolkit
date: 2020-07-08T10:55:39+00:00
author: Richard Brynteson
layout: post
permalink: /2020/07/08/teams-toolkit/
categories:
  - Microsoft Teams
  - Development
---

At Microsoft Build 2020, the Teams Development team announced the preview version of the Teams Toolkit.  I've been fortunate to see this go from napkin stage all the way to general availability.  I thought it would be fun to take it out for a quick test.  You can read all about on [Microsoft Docs](https://docs.microsoft.com/en-us/microsoftteams/platform/toolkit/visual-studio-code-overview).

## My Project

This should be a very easy project.  I have a project currently in process that spans multiple GitHub repositories.  We do all of issue tracking via GitHub.  For every project that has a user experience tied to it, we build in a very simple "feedback" form so user (internal, external, whomever) can project us with feature requests, bugs or whatever other thoughts they have.  Our project management team has asked to be able to see all of the GitHub issues but they don't want to have to browse to all of these repositories.  So using the Octokit.NET we built a REST endpoint that we can pass any number of repository names and it will return all of the issues that are open across all the collection.

So I want to build a channel tab that I can put inside the project so the project management team can see all the open issues in a single spot.

## Setup

To use the new Teams Toolkit, all you need to do is install it in Visual Studio Code.  The team announced at Build that a similar tool was coming for Visual Studio.  Head over to extensions in Visual Studio Code, search for Teams Toolkit and there you go.

<img src="https://theargylemvp.com/assets/images/07.08.2020.1.png" width="650" />

Next we need to create a new project.  Inside of Code you will see a new Teams icon on the left rail.  Go ahead and click on it and you will see a list of different options.

<img src="https://theargylemvp.com/assets/images/07.08.2020.2.png" />

Click on Create a new Teams app.  This will prompt you for a name for your application.  I'm going to call my application GitHubForTeams.

<img src="https://theargylemvp.com/assets/images/07.08.2020.3.png" />

It will prompt you where you want to save the workspace at.  Visual Studio Code is going to create a folder for you in this case.  So it's asking, what is the parent folder you want to store this in.  So in my case, I'm just putting it with all of my repositories.  After that you will be presented with a wizard to decide what items you want to add to your project.  In my case, I'm going to add a tab.

<img src="https://theargylemvp.com/assets/images/07.08.2020.4.png" width="650" />

Since I selected a tab in this instance, it's going to ask if I want a personal tab or a Group/Channel tab.  In my case we want to go with the Group/Channel tab.

<img src="https://theargylemvp.com/assets/images/07.08.2020.5.png" width="650" />

At this point in time, I'm finished.  It's all setup and I'm ready to go.  You will see a welcome screen, telling you exactly what you need to do to use the tool.  We can interact with our application both as a local host but also within Teams.  The readme does tell you what you need here but I'll detail it with screenshots as well for you.

<img src="https://theargylemvp.com/assets/images/07.08.2020.6.png" width="650" />

So our first step is to run:

>>npm install

<img src="https://theargylemvp.com/assets/images/07.08.2020.7.png" width="650" />

This is going to take a bit of time as it downloads all of the necessary items for your project.  Remember, this isn't just about creating the Manifest, but instead we are going to create everything needed for this project including the website files and more.  Once npm install is done, run:

>>npm start

<img src="https://theargylemvp.com/assets/images/07.08.2020.8.png" />

Now that you have started your application you can open it in the browser.  When you first go the page, you will met with a Connection Error.

<img src="https://theargylemvp.com/assets/images/07.08.2020.9.png" />

Remember, the read me tells you need to accept the local cert.  Yes, you could use the Advanced Button and simply "continue" for the browser but this won't work then for Teams.  So you need to actually install the cert.  That process is thankfully pretty easy.  I'm using Edge 85 for this.  Same instructions work for any version of Chrome as well.

1. Open the Chrome Developer Tools window F12 or you can use (ctrl + shift + i)
2. Go to the Security tab
3. Click on View Certificate.  Click on the Details tab and choose "Copy to File".  This is going to walk you through a wizard, you can hit next-next-next through the entire wizard.  At the end, you will be asked to name the certificate.  Save it somewhere you will know.

<img src="https://theargylemvp.com/assets/images/07.08.2020.13.png" width="650" />

<img src="https://theargylemvp.com/assets/images/07.08.2020.10.png" />

Now we need to install this into your computer.

1. Go to where you have the cert, right-click and choose Install Certificate.
2. Choose Local Machine.  It will then ask where you want to store the certificate, pick Trusted Root Certificate Authorities.
3. You can now click next-next-next and finish.  It should tell you the certificate was imported successfully.

<img src="https://theargylemvp.com/assets/images/07.08.2020.11.png" />

<img src="https://theargylemvp.com/assets/images/07.08.2020.12.png" />

Now you can try to go back to your webpage.  However, you will most likely notice that you are still getting a certificate error.  Why?  Because the address in Code tells you to go to https://10.0.3.209:3000/.  But 10.0.3.209 is my IP Address and that will NEVER be on a certificate.  Change it to https://localhost:3000/.  The error will be gone.

When you visit the webpage, it is complete blank!  This is because it's not in Teams but just a browser.  If you try to go to https://localhost:3000/tab/ it will tell you you are not in Teams.  So now let's get this into Teams.

First, you need to make sure your Teams client is configured for Developer Preview mode.   All you need to do is select your Profile picture, choose About and select Developer Preview.

<img src="https://theargylemvp.com/assets/images/07.08.2020.14.png" />

It will ask if you want to switch to Developer Preview and simply say yes.

<img src="https://theargylemvp.com/assets/images/07.08.2020.15.png" />

From the Welcome Page, click on the Edit App Package.  If you have not signed in previously, you will be met with a large "Sign In" page.

<img src="https://theargylemvp.com/assets/images/07.08.2020.16.png" width="650" />

Go ahead and sign-in.  If this really is your first time, you will be met with an Azure Authentication page to give your profile permission to the Teams App Configuration tool.  I'm not a huge fan that it says "unverified" but it is safe.  Go ahead and click Accept.

<img src="https://theargylemvp.com/assets/images/07.08.2020.17.png" />

NOTE: If your organization has disabled individual permissions to be granted in Azure, you will be told that your do not have sufficient permissions and you need to contact your administrator.  They will need to put the Teams App Configuration tool on the allow list in Azure.

Next you need to configure your application.  By default, everything is going to be fill out for you.  You can modify these settings now if you want or you can do it later.  Its completely up to you.

<img src="https://theargylemvp.com/assets/images/07.08.2020.18.png" width="650" />

When I hit update, I got a message about a mismatch between my App Config and my Local Project.  In my case, it was just a trailing / was missing.  I tried using both of the two options presented, but the message just kept coming up.  So I went to App Details and changed the Website to https://localhost:3000/

<img src="https://theargylemvp.com/assets/images/07.08.2020.19.png" />

Now you can hit the Update button.  We now want to side-load this into our Teams client.  To accomplish that, we need to browse to the location of our project.  When we hit the Update button, under the hood the a development.zip file has been created in the .publish folder.

<img src="https://theargylemvp.com/assets/images/07.08.2020.20.png" />

We go back to our Teams client and click on the Apps in the lower left corner.  Choose Upload a Custom App and browser to the folder where the development.zip file is located.  And click Add to a team 

<img src="https://theargylemvp.com/assets/images/07.08.2020.21.png" width="650" />

Use the picker and select a team.  In my case I'm going to use my Testing Group | General.  Click Setup a tab.

<img src="https://theargylemvp.com/assets/images/07.08.2020.22.png" width="650" />

Now we just need to add the tab to the channel.  So click the + like you normally would, find your new GitHubForTeams app and add it.

<img src="https://theargylemvp.com/assets/images/07.08.2020.23.png" />

And that is it.  You should now have your new tab added to Teams!  In my next post, we are going to go through what Microsoft has pre-built for you, add the stuff we need to complete this application and do some testing.

<img src="https://theargylemvp.com/assets/images/07.08.2020.24.png" width="650" />