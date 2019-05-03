---
id: 220
title: Understanding Voice Routing | Dialing Behaviors
date: 2013-04-07T22:44:27+00:00
author: admin@gcmtotalsolutions.com
layout: post
guid: http://masteringlync.com/?p=220
permalink: /2013/04/07/understanding-voice-routing-dialing-behaviors/
st_layout_box:
  - right
panels_data:
  - 'a:0:{}'
categories:
  - Lync Server 2013
tags:
  - Routing
---
It has been a little over a year since I first saw this PowerPoint slide presented by Doug Lawty and on that day my &#8220;Lync life&#8221; changed completely.  Seeing this slide made everything click in terms of Lync routing and I&#8217;ve become obsessed with understanding exactly how the routing engine works, how to trace calls and understanding exactly what is going on under the hood.

[<img class="alignnone wp-image-1034 size-column1-1/4" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/04/voice1_thumb1-687x285.jpg?resize=687%2C285&#038;ssl=1" alt="voice1_thumb" width="687" height="285" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/04/voice1_thumb1.jpg)

Slide from <span style="font-family: Verdana">TechEd</span> 2012 &#8211; <http://channel9.msdn.com/Events/TechEd/NorthAmerica/2012/EXL318>

Why post this now?  Over the past few weeks I&#8217;ve been trolling the Microsoft Forums for Lync 2010 and Lync 2013 and there have been a large number of questions on how routing of calls works.  So I starting doing some searching and couldn&#8217;t find any that really broke down the routing process in great detail so I figured this would be a good time to do that.  Over the course of the next few blog posts I&#8217;m going to break down each of these items in terms of routing, what components to use to trace the call path and what that would look like.

Before we start breaking down the call flow there is one important thing to note about the diagram above.  The flow starts in the upper right-corner with User Initiates Call but this call flow is exactly the same for both outbound and inbound dialing.  So for this first post we will focus on the Dialing Behaviors portion (top half) of an Outbound Call from the Lync.  Here are the posts we will see in detail:

  *   * Investigating Dialing – [Dialing Behaviors](http://masteringlync.com/2013/04/07/understanding-voice-routing-dialing-behaviors/) &#8211; This Post
      * Investigating Dialing – [Routing & Authorization](http://masteringlync.com/2013/04/11/understanding-voice-routing-routing-authorization/)
      * Investigating Dialing – [Dialing Behaviors of an Inbound Call](http://masteringlync.com/2013/05/31/understanding-voice-routing-inbound-routing/) 
      * Investigating Dialing – [Inter Trunk Dialing](http://masteringlync.com/2013/06/07/inter-trunk-routing-deep-dive/)

**Outbound Call &#8211; User Initiates Call**

The first box is where the magic starts.  The user dials a phone number in the Lync Client and we have to make the very first decision &#8211; is this a SIP URI or anything else.  If the number is a SIP URI (anything@anything) the client will immediately give you the option to dial that number.  When the number is dialed it will add a ;user=phone to the end of the dialed number.

[<img class="alignnone size-full wp-image-1040" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/04/voice71.png?resize=347%2C177&#038;ssl=1" alt="voice7" width="347" height="177" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/04/voice71.png?w=347&ssl=1 347w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/04/voice71.png?resize=300%2C153&ssl=1 300w" sizes="(max-width: 347px) 100vw, 347px" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/04/voice71.png)

Here we can see the client will attempt to call notarealname@masteringlync.com bypassing all call routing of the diagram and immediately search for that SIP URI within the system.  If the SIP URI is an internal SIP Address, it will search it&#8217;s local database for a match and route and if the SIP URI is external, the call will be routed to the edge server.

To see what happens I will ensure that tracing is enabled within my Lync client open the log file that is created in the C:UsersRichardAppDataLocalMicrosoftOffice15.0LyncTracing folder.  When I open up the log I find this message:

[<img class="alignnone size-full wp-image-1036" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/04/voice31.png?resize=502%2C99&#038;ssl=1" alt="voice3" width="502" height="99" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/04/voice31.png?w=502&ssl=1 502w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/04/voice31.png?resize=300%2C59&ssl=1 300w" sizes="(max-width: 502px) 100vw, 502px" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/04/voice31.png)

Here you can see from my work domain it is attempting to do a lookup for the masteringlync.com domain (on the edge) and because I don&#8217;t have any DNS records published for that domain, it returns a 404 Not Found:

[<img class="alignnone size-full wp-image-1037" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/04/voice41.png?resize=727%2C25&#038;ssl=1" alt="voice4" width="727" height="25" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/04/voice41.png?w=727&ssl=1 727w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/04/voice41.png?resize=300%2C10&ssl=1 300w" sizes="(max-width: 727px) 100vw, 727px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/04/voice41.png)

So now we understand how the SIP URI lookup works let&#8217;s look at the other leg of this call.  Let&#8217;s pretend the user dials <span class="baec5a81-e4d6-4674-97f3-e9220f0136c1">952-555-1111</span> for the number.  In this case the call isn&#8217;t a valid SIP URI so we would route to the next step.

**Emergency Call**

In this step we are going to investigate if this is an emergency call.  This is done based on the location profile assigned to the user and the emergency numbers defined within that location policy.

When a user logs into Lync, via in-band provisioning, the details of the location policy are passed to the client.  So again using snooper and looking at the same log file we see these settings passed to the workstation.  First we have a subscribe message:

[<img class="alignnone size-full wp-image-1038" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/04/voice51.png?resize=659%2C140&#038;ssl=1" alt="voice5" width="659" height="140" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/04/voice51.png?w=659&ssl=1 659w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/04/voice51.png?resize=300%2C64&ssl=1 300w" sizes="(max-width: 659px) 100vw, 659px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/04/voice51.png)

And then the response:

[<img class="alignnone size-full wp-image-1039" src="https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/04/voice61.png?resize=696%2C219&#038;ssl=1" alt="voice6" width="696" height="219" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/04/voice61.png?w=696&ssl=1 696w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/04/voice61.png?resize=300%2C94&ssl=1 300w" sizes="(max-width: 696px) 100vw, 696px" data-recalc-dims="1" />](https://i2.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/04/voice61.png)

So here we can see that an emergency number is considered to be 911 or by the Emergency Dial Mask 911 or 211.  If the number we had dialed was either 911 or 211 we would jump directly to the PSTN Usage &#8211; in this case 911 Emergency.

However in this case we are dialing <span class="baec5a81-e4d6-4674-97f3-e9220f0136c1">952-555-1111<a style="margin: 0px;border: currentColor;width: 16px;height: 16px;overflow: hidden;vertical-align: middle;float: none" title="Call: 952-555-1111" href="#"><img style="margin: 0px;border: currentColor;width: 16px;height: 16px;overflow: hidden;vertical-align: middle;float: none" title="Call: 952-555-1111" src="image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAYAAAAf8/9hAAAACXBIWXMAAA7EAAAOxAGVKw4bAAAKT2lDQ1BQaG90b3Nob3AgSUNDIHByb2ZpbGUAAHjanVNnVFPpFj333vRCS4iAlEtvUhUIIFJCi4AUkSYqIQkQSoghodkVUcERRUUEG8igiAOOjoCMFVEsDIoK2AfkIaKOg6OIisr74Xuja9a89+bN/rXXPues852zzwfACAyWSDNRNYAMqUIeEeCDx8TG4eQuQIEKJHAAEAizZCFz/SMBAPh+PDwrIsAHvgABeNMLCADATZvAMByH/w/qQplcAYCEAcB0kThLCIAUAEB6jkKmAEBGAYCdmCZTAKAEAGDLY2LjAFAtAGAnf+bTAICd+Jl7AQBblCEVAaCRACATZYhEAGg7AKzPVopFAFgwABRmS8Q5ANgtADBJV2ZIALC3AMDOEAuyAAgMADBRiIUpAAR7AGDIIyN4AISZABRG8lc88SuuEOcqAAB4mbI8uSQ5RYFbCC1xB1dXLh4ozkkXKxQ2YQJhmkAuwnmZGTKBNA/g88wAAKCRFRHgg/P9eM4Ors7ONo62Dl8t6r8G/yJiYuP+5c+rcEAAAOF0ftH+LC+zGoA7BoBt/qIl7gRoXgugdfeLZrIPQLUAoOnaV/Nw+H48PEWhkLnZ2eXk5NhKxEJbYcpXff5nwl/AV/1s+X48/Pf14L7iJIEyXYFHBPjgwsz0TKUcz5IJhGLc5o9H/LcL//wd0yLESWK5WCoU41EScY5EmozzMqUiiUKSKcUl0v9k4t8s+wM+3zUAsGo+AXuRLahdYwP2SycQWHTA4vcAAPK7b8HUKAgDgGiD4c93/+8//UegJQCAZkmScQAAXkQkLlTKsz/HCAAARKCBKrBBG/TBGCzABhzBBdzBC/xgNoRCJMTCQhBCCmSAHHJgKayCQiiGzbAdKmAv1EAdNMBRaIaTcA4uwlW4Dj1wD/phCJ7BKLyBCQRByAgTYSHaiAFiilgjjggXmYX4IcFIBBKLJCDJiBRRIkuRNUgxUopUIFVIHfI9cgI5h1xGupE7yAAygvyGvEcxlIGyUT3UDLVDuag3GoRGogvQZHQxmo8WoJvQcrQaPYw2oefQq2gP2o8+Q8cwwOgYBzPEbDAuxsNCsTgsCZNjy7EirAyrxhqwVqwDu4n1Y8+xdwQSgUXACTYEd0IgYR5BSFhMWE7YSKggHCQ0EdoJNwkDhFHCJyKTqEu0JroR+cQYYjIxh1hILCPWEo8TLxB7iEPENyQSiUMyJ7mQAkmxpFTSEtJG0m5SI+ksqZs0SBojk8naZGuyBzmULCAryIXkneTD5DPkG+Qh8lsKnWJAcaT4U+IoUspqShnlEOU05QZlmDJBVaOaUt2ooVQRNY9aQq2htlKvUYeoEzR1mjnNgxZJS6WtopXTGmgXaPdpr+h0uhHdlR5Ol9BX0svpR+iX6AP0dwwNhhWDx4hnKBmbGAcYZxl3GK+YTKYZ04sZx1QwNzHrmOeZD5lvVVgqtip8FZHKCpVKlSaVGyovVKmqpqreqgtV81XLVI+pXlN9rkZVM1PjqQnUlqtVqp1Q61MbU2epO6iHqmeob1Q/pH5Z/YkGWcNMw09DpFGgsV/jvMYgC2MZs3gsIWsNq4Z1gTXEJrHN2Xx2KruY/R27iz2qqaE5QzNKM1ezUvOUZj8H45hx+Jx0TgnnKKeX836K3hTvKeIpG6Y0TLkxZVxrqpaXllirSKtRq0frvTau7aedpr1Fu1n7gQ5Bx0onXCdHZ4/OBZ3nU9lT3acKpxZNPTr1ri6qa6UbobtEd79up+6Ynr5egJ5Mb6feeb3n+hx9L/1U/W36p/VHDFgGswwkBtsMzhg8xTVxbzwdL8fb8VFDXcNAQ6VhlWGX4YSRudE8o9VGjUYPjGnGXOMk423GbcajJgYmISZLTepN7ppSTbmmKaY7TDtMx83MzaLN1pk1mz0x1zLnm+eb15vft2BaeFostqi2uGVJsuRaplnutrxuhVo5WaVYVVpds0atna0l1rutu6cRp7lOk06rntZnw7Dxtsm2qbcZsOXYBtuutm22fWFnYhdnt8Wuw+6TvZN9un2N/T0HDYfZDqsdWh1+c7RyFDpWOt6azpzuP33F9JbpL2dYzxDP2DPjthPLKcRpnVOb00dnF2e5c4PziIuJS4LLLpc+Lpsbxt3IveRKdPVxXeF60vWdm7Obwu2o26/uNu5p7ofcn8w0nymeWTNz0MPIQ+BR5dE/C5+VMGvfrH5PQ0+BZ7XnIy9jL5FXrdewt6V3qvdh7xc+9j5yn+M+4zw33jLeWV/MN8C3yLfLT8Nvnl+F30N/I/9k/3r/0QCngCUBZwOJgUGBWwL7+Hp8Ib+OPzrbZfay2e1BjKC5QRVBj4KtguXBrSFoyOyQrSH355jOkc5pDoVQfujW0Adh5mGLw34MJ4WHhVeGP45wiFga0TGXNXfR3ENz30T6RJZE3ptnMU85ry1KNSo+qi5qPNo3ujS6P8YuZlnM1VidWElsSxw5LiquNm5svt/87fOH4p3iC+N7F5gvyF1weaHOwvSFpxapLhIsOpZATIhOOJTwQRAqqBaMJfITdyWOCnnCHcJnIi/RNtGI2ENcKh5O8kgqTXqS7JG8NXkkxTOlLOW5hCepkLxMDUzdmzqeFpp2IG0yPTq9MYOSkZBxQqohTZO2Z+pn5mZ2y6xlhbL+xW6Lty8elQfJa7OQrAVZLQq2QqboVFoo1yoHsmdlV2a/zYnKOZarnivN7cyzytuQN5zvn//tEsIS4ZK2pYZLVy0dWOa9rGo5sjxxedsK4xUFK4ZWBqw8uIq2Km3VT6vtV5eufr0mek1rgV7ByoLBtQFr6wtVCuWFfevc1+1dT1gvWd+1YfqGnRs+FYmKrhTbF5cVf9go3HjlG4dvyr+Z3JS0qavEuWTPZtJm6ebeLZ5bDpaql+aXDm4N2dq0Dd9WtO319kXbL5fNKNu7g7ZDuaO/PLi8ZafJzs07P1SkVPRU+lQ27tLdtWHX+G7R7ht7vPY07NXbW7z3/T7JvttVAVVN1WbVZftJ+7P3P66Jqun4lvttXa1ObXHtxwPSA/0HIw6217nU1R3SPVRSj9Yr60cOxx++/p3vdy0NNg1VjZzG4iNwRHnk6fcJ3/ceDTradox7rOEH0x92HWcdL2pCmvKaRptTmvtbYlu6T8w+0dbq3nr8R9sfD5w0PFl5SvNUyWna6YLTk2fyz4ydlZ19fi753GDborZ752PO32oPb++6EHTh0kX/i+c7vDvOXPK4dPKy2+UTV7hXmq86X23qdOo8/pPTT8e7nLuarrlca7nuer21e2b36RueN87d9L158Rb/1tWeOT3dvfN6b/fF9/XfFt1+cif9zsu72Xcn7q28T7xf9EDtQdlD3YfVP1v+3Njv3H9qwHeg89HcR/cGhYPP/pH1jw9DBY+Zj8uGDYbrnjg+OTniP3L96fynQ89kzyaeF/6i/suuFxYvfvjV69fO0ZjRoZfyl5O/bXyl/erA6xmv28bCxh6+yXgzMV70VvvtwXfcdx3vo98PT+R8IH8o/2j5sfVT0Kf7kxmTk/8EA5jz/GMzLdsAAAAgY0hSTQAAeiUAAICDAAD5/wAAgOkAAHUwAADqYAAAOpgAABdvkl/FRgAAAaNJREFUeNqk009oz3Ecx/HH9/P9/YQRWSi2MGmNrebkpJgSLluKEw5Kk5zUsgMOTrO42MUVxUWRg+LgaPWbUGq7rRk7sExKNPt9vx8H32l921z2vrw+n3fvz/Pz/vP5JDFGy7HKwk1/fz/sR09VNjAZG6fv1buQHcZm3P8vAMfwCKswncoHCn8nbuAratiEUQglwLXiMJz/FhvWYC9OFb4htBV6B9VyBtsKzRJxbDhvbSS5i1kcQo736MNrjJcBeaG3MuFyInagA0fwckHcGzzGiXIJU38bk1U+x3W+x7U/yOvYU4prwG5MlAHPoS4925TMNDeHLxNUh0gGcRC9eFqk34TBMuAhfmL9b5Vz3elIbAmfLq02d4HQhbc4gBYcxUgZMFrcIJecrsgaT6avdl6sPqu1hw9XSWs4U5Q6udgYFfP+he25MEysVWQvutORLcfTYax4gn3z/VoM8A7Xi/WuKNmQCRujpLc1TGkP46hOI1sKADdxG3PzjkzoTNCT1rSFiX+PuLIEoI4rGC+6/xEPMsFKs7YmM8bsAMlyf2OwTPszAMZMeayGCpJVAAAAAElFTkSuQmCC" alt="" /></a></span> so this would not be an emergency number and therefore we must move onto the next step.

**Global**

This step is very easy but incredibly critical in the routing process.   As shown above in the call flow if a number is considered global it bypasses all dial plans and goes directly to Reverse Number Lookup (RNL).  A number that is global is one that starts with a +.  This simple check can cause all sorts of problems with a deployment as is evidence based on the number of posts and questions I&#8217;ve seen about it.  Have you ever wondered why your dial plans are ignored on inbound normalization when the upstream PBX/IP Gateway sends you a number with a + preprend to it?  It is because the number is considered &#8220;global&#8221; and we have bypassed all of your dial plans.

So when a user dials a number from their Lync client it will automatically normalize the number based on rules assigned in the dial plan.  (This is also true if I were to click-to-call someone from my Lync client as well.)

&nbsp;

[<img class="alignnone size-full wp-image-1040" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/04/voice71.png?resize=347%2C177&#038;ssl=1" alt="voice7" width="347" height="177" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/04/voice71.png?w=347&ssl=1 347w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/04/voice71.png?resize=300%2C153&ssl=1 300w" sizes="(max-width: 347px) 100vw, 347px" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/04/voice71.png)

So when the call is made the front-end server will determine the number as &#8220;global&#8221; and no additional processing will be done.  This can be seen if you do a trace that includes the TranslationApplication component.

[<img class="alignnone size-full wp-image-1041" src="https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/04/voice8_thumb1.jpg?resize=800%2C53&#038;ssl=1" alt="voice8_thumb" width="800" height="53" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/04/voice8_thumb1.jpg?w=1020&ssl=1 1020w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/04/voice8_thumb1.jpg?resize=300%2C20&ssl=1 300w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/04/voice8_thumb1.jpg?resize=768%2C51&ssl=1 768w" sizes="(max-width: 800px) 100vw, 800px" data-recalc-dims="1" />](https://i0.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/04/voice8_thumb1.jpg)

If you are using CLS Logging in Lync 2013 this is included in the IncomingAndOutgoingCall scenario or you can select TranslationApplciation from OCS Logger.  If you are going to use OCS Logger and Lync 2013 make sure to read my good friend Jon McKinney&#8217;s blog post otherwise you won&#8217;t see anything in Snooper.

<http://blog.lyncdialog.com/2013/03/inboundrouting-and-outboundrouting-does.html>

So in this case the call is considered &#8220;global&#8221; and we move onto the next step.

**Reverse Number Lookup**

In this step we determine if we are going to route the call using Inbound or Outbound routing.  Inbound routing means that RNL matched an users LineURI exactly and we must route the call to an internal end-point.  Outbound routing means that no user information was matched and we must route out of Lync.

I&#8217;ve never found a really good logging process to determine if a number matches RNL other than looking at the log files.  For example, the first picture below is my dialing the phone number of an internal Lync user:

[<img class="alignnone size-full wp-image-1042" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/04/voice91.png?resize=443%2C109&#038;ssl=1" alt="voice9" width="443" height="109" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/04/voice91.png?w=443&ssl=1 443w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/04/voice91.png?resize=300%2C74&ssl=1 300w" sizes="(max-width: 443px) 100vw, 443px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/04/voice91.png)

And the second is a call to an external user:

[<img class="alignnone size-full wp-image-1043" src="https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/04/voice101.png?resize=374%2C108&#038;ssl=1" alt="voice10" width="374" height="108" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/04/voice101.png?w=374&ssl=1 374w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2013/04/voice101.png?resize=300%2C87&ssl=1 300w" sizes="(max-width: 374px) 100vw, 374px" data-recalc-dims="1" />](https://i1.wp.com/masteringlync.gcmtotalsolutions.com/wp-content/uploads/sites/2/2013/04/voice101.png)

You can see in the first picture that the internal call will start down the InboundRouting competent from Snooper and the PSTN call appears under OutboundRouting.  This is the best way I&#8217;ve found to determine if RNL was matched or not.

So at this point in time our call has been made and arrived at RNL and needs to be forwarded to the next step Vacant Number Lookup but we will save that for our next blog post.  But you might be asking yourself, what about that section of the call flow that included the Dial Plan, Normalization Rules and more (the upper left side of the slide).  We are going to detail this specifically in the inbound call scenario as that is the most frequent place you see those used however that is not to say they will not ever be used in an outbound call.  Most devices (Lync Client, Aries Phones, others) download the normalization rules to the local device and because we normalize everything to E164 (with the plus) we will most likely skip over these items in many of our outbound calls.

Questions or comments are always welcome.