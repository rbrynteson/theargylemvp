---
title: My First Teams App
date: 2020-03-13T13:11:39+00:00
author: Richard Brynteson
layout: post
permalink: /2020/03/13/my-first-teams-app/
categories:
  - Microsoft Teams
  - Development
---
<script type="application/ld+json">
{
    "@context": "https://schema.org",
    "@type": "Article",
    "mainEntityOfPage": {
        "@type": "WebPage",
        "@id": "https://theargylemvp.com/2020/03/13/my-first-teams-app/"
    },
    "headline": "My First Teams App",
    "alternativeHeadline": "My First Teams App",
    "url": "https://theargylemvp.com/2020/03/13/my-first-teams-app/",
    "datePublished": "Fri Mar 13 2020",
    "dateModified": "Fri Mar 13 2020",
    "description": "Looking at what it will take to create your first teams application.",
    "author": {
        "@type": "Person",
        "name": "Richard Brynteson"
    },
    "publisher": {
        "@type": "Organization",
        "name": "TheArgyleMVP",
        "logo": {
            "@type": "ImageObject",
            "url": "https://theargylemvp.com/assets/images/TheArgyleMVPLogo.png",
            "width": 720,
            "height": 120
        }
    }
}
</script>

This has been on my "to-do" for nearly a year and I've decided that I can no longer wait any longer and the time is now.  So today I'm going to start working on My First Teams App - Validator for Teams.

## What Is Validator?

Many years ago I wrote a web app to help individuals and customers with their Skype for Business Deployments.  This app is still available(https://skypevalidator.com/) for those who are on Lync/Skype but one has to admit that it's use has gone down with Microsoft Teams over the years.  One of the items I wrote as part of Skype Validator was a tool to do simple HTTP/HTTPS Checks, Certificate Checks, Ping, etc.

<img src="https://theargylemvp.com/assets/images/03132020-image1.png" width="650" />

These checks could be run ad-hoc or scheduled and run all the time.  So I've decided that it's time to take this tool, modernize it, and put it into Teams.

## The Plan

In order to have write an app we need to set some ground rules of what will be in the app, how it will function and more.  So here is my list of to-do's for my new app.

- Open Source: There is both a front-end and back-end we will need to open source as part of this.
- Language: We will use ReactJS for the front-end and WebAPI2.0 for the backend.
- Personal Tab: This will be where you can run ad-hoc tests.
- Team Tab: This is where you will be able to run reoccurring tests and report back.
- Bot: Not sure yet: Need to think if this would be helpful.
- Messaging Extension: I love Messaging Extensions.  I was thinking maybe searching for past down events.
- Channel Messages: Yep, need to have them.  System down alerts and such.

So that is my current plan.  My hope is to weekly post where I'm at, what we accomplished and how you can also follow along and learn.  Let's see if I can keep up the weekly posts.
