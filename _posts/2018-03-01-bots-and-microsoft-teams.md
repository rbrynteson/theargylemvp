---
id: 1483
title: Bots and Microsoft Teams
date: 2018-03-01T11:13:13+00:00
author: Richard Brynteson
layout: post
guid: http://masteringlync.com/?p=1483
permalink: /2018/03/01/bots-and-microsoft-teams/
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
image: https://masteringlync.com/wp-content/uploads/sites/2/2018/03/1.png
categories:
  - Bots
  - Featured
  - Microsoft Teams
---
So I&#8217;ve been tasked with creating some fun bots for work.  So when I jumped into the Azure Portal and created my first bot I noticed a lot of different &#8220;sample&#8221; projects that were not there before.  One in particular jumped out to me, the QnAMaker.  So I decided to spend a bunch of time before our last Users Group meeting and create one.

[NOTE: Since I&#8217;ve been writing this blog post over the last week a real developer &#8211; <a href="https://blog.thoughtstuff.co.uk/2018/03/building-an-faq-bot-using-azure-and-qnamaker-without-writing-any-code-part-1/" target="_blank" rel="noopener">Tom Morgan</a> has posted on the same subject &#8211; so go read his blog about this because honestly he is 1000% better at this than me.  Did I mention, he is a developer!]

&nbsp;

[NOTE AGAIN: Seriously, why are you still reading this post?  <a href="https://blog.thoughtstuff.co.uk/2018/03/building-an-faq-bot-using-azure-and-qnamaker-without-writing-any-code-part-1/" target="_blank" rel="noopener">Tom&#8217;s </a>has a video even and is a developer.  Only continue to read if you don&#8217;t know what you are doing and want to laugh.]

&nbsp;

So let&#8217;s create our bot.

Step One: Go login to the Azure Portal.  Sign up if you don&#8217;t have an account.  Bots are super cheap to run so you can do this nearly free I think.

Step Two: Click on Add Resource, click AI + Cognitive Services and choose Web App Bot

<img class="alignnone wp-image-1484 size-large" src="https://masteringlync.com/wp-content/uploads/2018/03/1.png?resize=800%2C569&#038;ssl=1" alt="" width="800" height="569"  /> 

Step Three: Enter the details of your bot.  A few things I learned while I was doing this.  When you are done, click Create.  It will take a few minutes.

  * Make sure to select the Question and Answer from the Bot Template
  * Create a new storage location.  I tried several times selecting an existing storage group and it failed every time.
  * Make sure you write down that App ID and Password you create at the bottom.  Not sure if or when you might need that password again but it is very clear that you never get to see it again.

<img class="alignnone wp-image-1485 size-large" src="https://masteringlync.com/wp-content/uploads/2018/03/2.png?resize=800%2C583&#038;ssl=1" alt="" width="800" height="583" /> 

Step Four: Create your QnA from Microsoft&#8217;s QnA Service.  Head over to https://qnamaker.ai/ and login.  When you are there, click on New Service and fill in the form.

<img class="alignnone wp-image-1486 size-large" src="https://masteringlync.com/wp-content/uploads/2018/03/3.png?resize=800%2C434&#038;ssl=1" alt="" width="800" height="434" /> 

Step Five: Add some questions and answers.  There is an option to crawl your website in the above step.  I tried SO MANY different pages and none of them worked.  So I&#8217;m not sure what format they are looking for but I don&#8217;t have it.

<img class="alignnone wp-image-1487 size-large" src="https://masteringlync.com/wp-content/uploads/2018/03/4.png?resize=800%2C131&#038;ssl=1" alt="" width="800" height="131" /> 

Step Six: Click publish at the top of the page.  You will see a page like the below.  It will contain your Subscription Key and Knowledgebase ID.

<img class="alignnone wp-image-1488 size-large" src="https://masteringlync.com/wp-content/uploads/2018/03/5.png?resize=800%2C286&#038;ssl=1" alt="" width="800" height="286"  /> 

Step Seven: Go back to the Azure Portal and your bot should be done now.  Go to Application Settings and enter in the Subscription Key and Knowledgebase ID.  You will need to scroll way down.

<img class="alignnone wp-image-1490 size-large" src="https://masteringlync.com/wp-content/uploads/2018/03/6.png?resize=800%2C397&#038;ssl=1" alt="" width="800" height="397" /> 

Step Eight: Your bot should work now.  So you can head over to Test in Web Chat and it should work.  So lets make it available in Microsoft Teams now.

<img class="alignnone wp-image-1491 size-large" src="https://masteringlync.com/wp-content/uploads/2018/03/7.png?resize=800%2C496&#038;ssl=1" alt="" width="800" height="496" /> 

Step Nine: In the Azure Portal, click on Channels under your bot.

<img class="alignnone wp-image-1492 size-large" src="https://masteringlync.com/wp-content/uploads/2018/03/8.png?resize=800%2C504&#038;ssl=1" alt="" width="800" height="504" /> 

Then accept the license agreement.

<img class="alignnone wp-image-1493 size-large" src="https://masteringlync.com/wp-content/uploads/2018/03/9.png?resize=800%2C428&#038;ssl=1" alt="" width="800" height="428"  /> 

Step Ten: Lets go chat with our new bot in Microsoft Teams.  Remember that App ID that I told you to write down in Step Three?  Go start a new chat in Teams with that App ID.

<img class="alignnone wp-image-1494 size-large" src="https://masteringlync.com/wp-content/uploads/2018/03/10.png?resize=800%2C130&#038;ssl=1" alt="" width="800" height="130" /> 

What&#8217;s next?  Let&#8217;s make it so your bot says hello back to you when you start a chat with it.  That will be the next step in our fancy new bot.  Then we will figure out how to distribute it to your business.

&nbsp;

&nbsp;