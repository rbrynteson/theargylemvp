---
id: 1715
title: 'The End is Near: Exchange UM Online Death March'
date: 2019-02-08T14:59:37+00:00
author: Richard Brynteson
layout: post
guid: http://masteringlync.com/?p=1715
permalink: /2019/02/08/the-end-is-near-exchange-um-online-death-march/
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
image: https://masteringlync.com/wp-content/uploads/sites/2/2019/02/Voicemail20to20Email.jpg
categories:
  - Exchange Online
  - Skype for Business
---
The news we all knew was coming just didn&#8217;t know has finally landed. Microsoft Exchange Unified Messaging Online is finally going to be killed and everyone is going to be moved (some may call it forced) to Azure Voice Mail Services.

The blog post can be found on the [You Had Me At EHLO](https://blogs.technet.microsoft.com/exchange/2019/02/08/retiring-unified-messaging-in-exchange-online/) and contains tons of information about the upcoming migration. There is a second article on the [Docs](https://docs.microsoft.com/en-us/SkypeForBusiness/plan/exchange-unified-messaging-online-migration-support) website that describes the differences between the two products and that is where I want to spend some times looking at the features. We know that Microsoft is not in the voicemail service but its important to get users ready for what could be a bumpy ride for some. So let&#8217;s look at some of the potential issues:

Lync 2010: This one is easy. If you are still on Lync 2010 you have until February 2020 to migration to SfB 2015/2019 or to Teams (or I guess to another platform) as there is no Azure Voicemail Support for you. It&#8217;s time to start the migration. If you need help, ask me, bet I can install SfB 15 in under 8 hours. 🙂

Third Party PBX Support: We don&#8217;t need to beat a dead horse but the entire world knew of this problem. So if you are a large, complex, multi-site PBX deployments and SfB 15/19 you already know you need to either accelerate your PBX migration, go back and redeploy your PBX voicemail solution or find a third party solution like AVST. The clock is ticking and it&#8217;s time to get moving.

PSTN Access to Voicemail: There is NONE! This is going to be a HUGE problem for many companies. We can pretend that people don&#8217;t like phones all we want but for some it&#8217;s simply not going to work. Good news is you can dial in from something like an authenticated mobile device to check your voicemail.

Record Greetings from Phone: This is actually going to be a bigger problem than people realize. There are MANY companies that have deployed 3rd Party IP Phones (think VVX, AudioCodes, etc.) and do NOT have a client deployed. For these customers they use SfB 15/19 like a traditional PBX. Unfortunately, from those devices you CANNOT record a greeting. You MUST dial from a client to record the greeting. Try it today, if you call into voicemail there is a &#8220;menu&#8221; with one option. To listen to new messages. I think this is low hanging fruit and it would be wise for Microsoft to change this behavior.

Disable Transcription: You can&#8217;t turn it off as a user anymore. Unfortunate but not the end of the world.

Voicemail Broadcast: Cool feature. I&#8217;m sure someone used it but I never saw it in the wild.

SMS Support. Most likely never used.

Play On Phone Support: It&#8217;s gone. Sure you can call into via a logged in IP Phone to listen to messages but there was something cool about being able to send it to a cell phone or something else. Again, it&#8217;s one of those we most likely can live without moments.

Protected Voicemail: I&#8217;m sure for some security conscience companies this is HUGE.

Fax: Not sure many used it. But again, those who did this is going to cost some money.

At the end of the day, it&#8217;s not a death blow to the Skype and Teams world but the lack of PSTN Access to Voicemail and the ability to change greetings directly from the phone is going to cause LOTS of problems for many large enterprises. Hopefully there is enough movement with those groups to address these two issues.