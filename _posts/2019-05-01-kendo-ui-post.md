---
id: 1783
title: Using KendoUI Template with External Files
date: 2019-05-01T07:31:39+00:00
author: Richard Brynteson
layout: post
guid: http://masteringlync.com/?p=1783
permalink: /2019/05/01/kendo-ui-post/
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
image: https://masteringlync.com/wp-content/uploads/sites/2/2018/11/code2.png
categories:
  - Uncategorized
---
Many years ago when I started my journey into the development world I was all about VB. Somewhere along that journey, I jumped over to C# because it made sense and all of my websites quickly transitioned to ASP.NET Web Forms. In order to make things look pretty I found UI for ASP.NET Ajax and many websites were born.

Over the past year I&#8217;ve been on a mission to move from the server to the client. Take advantage of newer technologies and features but one tool never left me in Telerik (now Progress). So although I&#8217;ve played with ReactJS, Angular and others, I keep coming back to Telerik and now we move to KendoUI for JQuery. Great libraries, ability to quickly create content without all of the crazy that goes along with React (for now). Thought for my normal readers it would be good to know why I&#8217;m posting about this because it doesn&#8217;t seem to make any sense on this blog.

So, what is the ask and the need. I really wanted to start using Kendo UI for my new SfB/Teams Portal I&#8217;m writing. And although I really love the new world of lots of JavaScript and objects there is something great about having externally referenced files for content. Templates. You know, those things we used since the dawn of time.

**Kendo UI with External Templates**﻿

The Telerik team has created a great starting point with their post about [External Template Loading](https://docs.telerik.com/kendo-ui/framework/templates/load-remote). And although at first glance you would think that is enough it really isn&#8217;t for a handful of reasons. So lets start with the finished code and explain what is happening in each item:

<pre class="brush: xml; title: ; notranslate" title="">&lt;!DOCTYPE html&gt;
&lt;html lang="en" class="h-100"&gt;
&lt;head&gt;
    &lt;meta charset="utf-8" /&gt;
    &lt;meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no"&gt;
    &lt;title&gt;&lt;/title&gt;

    &lt;!-- Kendo UI --&gt;
    &lt;link rel="stylesheet" href="/styles/kendo/kendo.common.min.css" /&gt;
    &lt;link rel="stylesheet" href="/styles/kendo/kendo.material.min.css" /&gt;
    &lt;link rel="stylesheet" href="/styles/kendo/kendo.material.mobile.min.css" /&gt;
    &lt;script src="/javascript/kendo/jquery.min.js"&gt;&lt;/script&gt;
    &lt;script src="/javascript/kendo/kendo.all.min.js"&gt;&lt;/script&gt;

    &lt;!-- Global Custom JS --&gt;
    &lt;!-- NOTE #1 --&gt;
    &lt;script src="/javascript/global/templateloader.js"&gt;&lt;/script&gt;


&lt;/head&gt;
&lt;body&gt;
    &lt;!-- Note #2 --&gt;
    &lt;div id="divContent"&gt;&lt;/div&gt;

    &lt;script type="text/javascript"&gt;

        // Note #3
        templateLoader.loadExtTemplate("/views/templates/training-files.htm");

        // Note #4
        //Subscribe to the event triggered when the templates are loaded
        //Do not load use templates before they are available
        $(document).bind("TEMPLATE_LOADED", function(e, path) {

            console.log('Templates loaded');

            //Compile and cache templates  
            // Note #5
            var template = kendo.template($("#TrainingFilesTemplate").html(),{useWithBlock:false});
            var data = [];
            var result = template(data);

            //Replace Data
            // Note #6
            $("#divContent").html(result);
        })
    &lt;/script&gt;
&lt;/body&gt;
&lt;/html&gt;
</pre>

The first part that is of value to look at is the templateloader.js. This is a straight copy/paste directly from the above mentioned post from Telerik.

<pre class="brush: jscript; title: ; notranslate" title="">//Creates a gloabl object called templateLoader with a single method "loadExtTemplate"
var templateLoader = (function ($, host) {
    //Loads external templates from path and injects in to page DOM
    return {
        //Method: loadExtTemplate
        //Params: (string) path: the relative path to a file that contains template definition(s)
        loadExtTemplate: function (path) {
            //Use jQuery Ajax to fetch the template file
            var tmplLoader = $.get(path)
                .success(function (result) {
                    //On success, Add templates to DOM (assumes file only has template definitions)
                    $("body").append(result);
                })
                .error(function (result) {
                    alert("Error Loading Templates -- TODO: Better Error Handling");
                })

            tmplLoader.complete(function () {
                //Publish an event that indicates when a template is done loading
                $(host).trigger("TEMPLATE_LOADED", [path]);
            });
        }
    };
})(jQuery, document);
</pre>

In it, we are writing a function to load a template from file based on a singular parameter and that is the file path. 

Note #2 is where we are going to put the content on the page. In our case, id=divContent.

Note #3 is where we actually load the file. We call the templateLoader function we added previously and specific the content of the file. In my case, the file content is simply a single paragraph. The key is the type and remember the ID.

<pre class="brush: xml; title: ; notranslate" title="">&lt;script type="text/x-kendo-template" id="TrainingFilesTemplate"&gt;
    &lt;p&gt;
        &lt;span class="PageLabel2" style="font-weight: bold;"&gt;CLIENT (WINDOWS, MOBILE, ETC.)&lt;/span&gt;
    &lt;/p&gt;
&lt;/script&gt;
</pre>

Note #4 is where we start to deviate from the provided text. A few reasons for this. First off, there is a straight up typo in the Telerik sample code. Load it and you will find a missing ) in the middle of the code. That will throw you at first.

There are two items of note I want to call out.

Note #5 is where we load the template file into a variable. If we don&#8217;t do this process of waiting, then the template file won&#8217;t load before we try to load it to the template variable and everything will fall apart. In this line we also see the useWithBlock set to false. There isn&#8217;t much for documentation on this but I did find this [post](https://www.telerik.com/forums/usewithblock-explanation) which describes it really well. Basically, if you are going to use this template a lot then you want to set to false for performance.

As you can see, in my case I set data to a blank array. You of course would add whatever data you need there. 

The very last line, Note #6, is where we actually replace the id=divContent with what we have in memory. And that is it. We have created our externally referenced template.

Happy Coding!