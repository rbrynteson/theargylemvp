---
id: 1602
title: 'Javascript: Export Web API (JSON) to CSV File'
date: 2018-09-13T11:45:23+00:00
author: Richard Brynteson
layout: post
guid: http://masteringlync.com/?p=1602
permalink: /2018/09/13/javascript-export-web-api-json-to-csv-file/
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
image: https://masteringlync.com/wp-content/uploads/sites/2/2018/09/Program-Code-Feature-Image.jpg
categories:
  - Development
---
If anyone has been following my blog or twitter or talking to me you will know that a good deal of my day is spent in Visual Studio.  I have a few different projects happening at any given time and since I&#8217;m not a developer it can take me a bit of time to get stuff done.  So a work project of mine has been updating our customer facing portal to rely less on server side processing and move it all to client-side.  This has lots of advantages but not in the least is to create a better experience for customers.

When we made this move, I started moving lots of large server-side processes to client side Web API 2 calls.  And with the move away from server-side I really wanted to retain some features that are easy to do in a server-side.  One of those is exporting data to CSV.  This is so simple on the server-side its crazy.  But on the client side it takes a bunch of work to make it happen.

Since I&#8217;m not a JavaScript expert I started by hunting around and I found someone who built a good part of this.  (<a href="https://medium.com/@danny.pule/export-json-to-csv-file-using-javascript-a0b7bc5b00d2" target="_blank" rel="noopener">CREDIT: Danny Pule</a>)

This got me three-quarters of the way there but I wanted to be able to consume the Web API.  All of the examples I had found were with defined data and I of course was going to be getting a JSON result.  So after a bit more research I found a way to take elements from Danny&#8217;s work, make it usable for myself and create a robust framework for my needs.

So what you need to update are:

  * Lines 53 to 55 with your CSV Header Names
  * Lines 58 with your Web API 2 Location
  * Lines 70 to 74 with your Header Information.  You need to make sure that these values match what is being returned from your Web/JSON return.

<div class="codecolorer-container javascript default" style="overflow:auto;white-space:nowrap;width:95%;height:300px;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />4<br />5<br />6<br />7<br />8<br />9<br />10<br />11<br />12<br />13<br />14<br />15<br />16<br />17<br />18<br />19<br />20<br />21<br />22<br />23<br />24<br />25<br />26<br />27<br />28<br />29<br />30<br />31<br />32<br />33<br />34<br />35<br />36<br />37<br />38<br />39<br />40<br />41<br />42<br />43<br />44<br />45<br />46<br />47<br />48<br />49<br />50<br />51<br />52<br />53<br />54<br />55<br />56<br />57<br />58<br />59<br />60<br />61<br />62<br />63<br />64<br />65<br />66<br />67<br />68<br />69<br />70<br />71<br />72<br />73<br />74<br />75<br />76<br />77<br />78<br />79<br />80<br />81<br />82<br />83<br />84<br />85<br />86<br />87<br />88<br />89<br />90<br />91<br />92<br />93<br />94<br />
        </div>
      </td>
      
      <td>
        <div class="javascript codecolorer">
          <span class="sy0"></</span>script<span class="sy0">></span><br /> <br /> <span class="kw1">function</span> convertToCSV<span class="br0">&#40;</span>objArray<span class="br0">&#41;</span> <span class="br0">&#123;</span><br /> <span class="kw1">var</span> array <span class="sy0">=</span> <span class="kw1">typeof</span> objArray <span class="sy0">!=</span> <span class="st0">'object'</span> <span class="sy0">?</span> JSON.<span class="me1">parse</span><span class="br0">&#40;</span>objArray<span class="br0">&#41;</span> <span class="sy0">:</span> objArray<span class="sy0">;</span><br /> <span class="kw1">var</span> str <span class="sy0">=</span> <span class="st0">''</span><span class="sy0">;</span><br /> <br /> <span class="kw1">for</span> <span class="br0">&#40;</span><span class="kw1">var</span> i <span class="sy0">=</span> <span class="nu0"></span><span class="sy0">;</span> i <span class="sy0"><</span> array.<span class="me1">length</span><span class="sy0">;</span> i<span class="sy0">++</span><span class="br0">&#41;</span> <span class="br0">&#123;</span><br /> <span class="kw1">var</span> line <span class="sy0">=</span> <span class="st0">''</span><span class="sy0">;</span><br /> <span class="kw1">for</span> <span class="br0">&#40;</span><span class="kw1">var</span> index <span class="kw1">in</span> array<span class="br0">&#91;</span>i<span class="br0">&#93;</span><span class="br0">&#41;</span> <span class="br0">&#123;</span><br /> <span class="kw1">if</span> <span class="br0">&#40;</span>line <span class="sy0">!=</span> <span class="st0">''</span><span class="br0">&#41;</span> line <span class="sy0">+=</span> <span class="st0">','</span><br /> <br /> line <span class="sy0">+=</span> <span class="st0">'"'</span> <span class="sy0">+</span> array<span class="br0">&#91;</span>i<span class="br0">&#93;</span><span class="br0">&#91;</span>index<span class="br0">&#93;</span> <span class="sy0">+</span> <span class="st0">'"'</span><span class="sy0">;</span><br /> <span class="br0">&#125;</span><br /> <br /> str <span class="sy0">+=</span> line <span class="sy0">+</span> <span class="st0">'<span class="es0">\r</span><span class="es0">\n</span>'</span><span class="sy0">;</span><br /> <span class="br0">&#125;</span><br /> <br /> <span class="kw1">return</span> str<span class="sy0">;</span><br /> <span class="br0">&#125;</span><br /> <br /> <span class="kw1">function</span> exportCSVFile<span class="br0">&#40;</span>headers<span class="sy0">,</span> items<span class="sy0">,</span> fileTitle<span class="br0">&#41;</span> <span class="br0">&#123;</span><br /> <span class="kw1">if</span> <span class="br0">&#40;</span>headers<span class="br0">&#41;</span> <span class="br0">&#123;</span><br /> items.<span class="me1">unshift</span><span class="br0">&#40;</span>headers<span class="br0">&#41;</span><span class="sy0">;</span><br /> <span class="br0">&#125;</span><br /> <br /> <span class="co1">// Convert Object to JSON</span><br /> <span class="kw1">var</span> jsonObject <span class="sy0">=</span> JSON.<span class="me1">stringify</span><span class="br0">&#40;</span>items<span class="br0">&#41;</span><span class="sy0">;</span><br /> <br /> <span class="kw1">var</span> csv <span class="sy0">=</span> <span class="kw1">this</span>.<span class="me1">convertToCSV</span><span class="br0">&#40;</span>jsonObject<span class="br0">&#41;</span><span class="sy0">;</span><br /> <br /> <span class="kw1">var</span> exportedFilenmae <span class="sy0">=</span> fileTitle <span class="sy0">+</span> <span class="st0">'.csv'</span> <span class="sy0">||</span> <span class="st0">'export.csv'</span><span class="sy0">;</span><br /> <br /> <span class="kw1">var</span> blob <span class="sy0">=</span> <span class="kw1">new</span> Blob<span class="br0">&#40;</span><span class="br0">&#91;</span>csv<span class="br0">&#93;</span><span class="sy0">,</span> <span class="br0">&#123;</span> type<span class="sy0">:</span> <span class="st0">'text/csv;charset=utf-8;'</span> <span class="br0">&#125;</span><span class="br0">&#41;</span><span class="sy0">;</span><br /> <span class="kw1">if</span> <span class="br0">&#40;</span>navigator.<span class="me1">msSaveBlob</span><span class="br0">&#41;</span> <span class="br0">&#123;</span> <span class="co1">// IE 10+</span><br /> navigator.<span class="me1">msSaveBlob</span><span class="br0">&#40;</span>blob<span class="sy0">,</span> exportedFilenmae<span class="br0">&#41;</span><span class="sy0">;</span><br /> <span class="br0">&#125;</span> <span class="kw1">else</span> <span class="br0">&#123;</span><br /> <span class="kw1">var</span> link <span class="sy0">=</span> document.<span class="me1">createElement</span><span class="br0">&#40;</span><span class="st0">"a"</span><span class="br0">&#41;</span><span class="sy0">;</span><br /> <span class="kw1">if</span> <span class="br0">&#40;</span>link.<span class="me1">download</span> <span class="sy0">!==</span> <span class="kw2">undefined</span><span class="br0">&#41;</span> <span class="br0">&#123;</span> <span class="co1">// feature detection</span><br /> <span class="co1">// Browsers that support HTML5 download attribute</span><br /> <span class="kw1">var</span> url <span class="sy0">=</span> URL.<span class="me1">createObjectURL</span><span class="br0">&#40;</span>blob<span class="br0">&#41;</span><span class="sy0">;</span><br /> link.<span class="me1">setAttribute</span><span class="br0">&#40;</span><span class="st0">"href"</span><span class="sy0">,</span> url<span class="br0">&#41;</span><span class="sy0">;</span><br /> link.<span class="me1">setAttribute</span><span class="br0">&#40;</span><span class="st0">"download"</span><span class="sy0">,</span> exportedFilenmae<span class="br0">&#41;</span><span class="sy0">;</span><br /> link.<span class="me1">style</span>.<span class="me1">visibility</span> <span class="sy0">=</span> <span class="st0">'hidden'</span><span class="sy0">;</span><br /> document.<span class="me1">body</span>.<span class="me1">appendChild</span><span class="br0">&#40;</span>link<span class="br0">&#41;</span><span class="sy0">;</span><br /> link.<span class="me1">click</span><span class="br0">&#40;</span><span class="br0">&#41;</span><span class="sy0">;</span><br /> document.<span class="me1">body</span>.<span class="me1">removeChild</span><span class="br0">&#40;</span>link<span class="br0">&#41;</span><span class="sy0">;</span><br /> <span class="br0">&#125;</span><br /> <span class="br0">&#125;</span><br /> <span class="br0">&#125;</span><br /> <br /> <span class="kw1">function</span> download<span class="br0">&#40;</span><span class="br0">&#41;</span> <span class="br0">&#123;</span><br /> <span class="kw1">var</span> headers <span class="sy0">=</span> <span class="br0">&#123;</span><br /> CustomerName<span class="sy0">:</span> <span class="st0">"Customer"</span><span class="sy0">,</span><br /> InvoiceEmail<span class="sy0">:</span> <span class="st0">"Invoice Address"</span><span class="sy0">,</span><br /> TechnicalEmail<span class="sy0">:</span> <span class="st0">"Technical Address"</span><br /> <span class="br0">&#125;</span><span class="sy0">;</span><br /> <br /> let url <span class="sy0">=</span> <span class="st0">'https://api.domain.com/GetContacts/{variable}/{variable}'</span><span class="sy0">;</span><br /> <br /> fetch<span class="br0">&#40;</span>url<span class="br0">&#41;</span><br /> .<span class="me1">then</span><span class="br0">&#40;</span>res <span class="sy0">=></span> res.<span class="me1">json</span><span class="br0">&#40;</span><span class="br0">&#41;</span><span class="br0">&#41;</span><br /> .<span class="me1">then</span><span class="br0">&#40;</span><span class="br0">&#40;</span>out<span class="br0">&#41;</span> <span class="sy0">=></span> <span class="br0">&#123;</span><br /> <br /> <span class="kw1">var</span> itemsNotFormatted <span class="sy0">=</span> <span class="br0">&#91;</span><span class="br0">&#93;</span><span class="sy0">;</span><br /> itemsNotFormatted <span class="sy0">=</span> out<span class="sy0">;</span><br /> <br /> <span class="kw1">var</span> itemsFormatted <span class="sy0">=</span> <span class="br0">&#91;</span><span class="br0">&#93;</span><span class="sy0">;</span><br /> <br /> <span class="co1">// format the data</span><br /> itemsNotFormatted.<span class="me1">forEach</span><span class="br0">&#40;</span><span class="br0">&#40;</span>item<span class="br0">&#41;</span> <span class="sy0">=></span> <span class="br0">&#123;</span><br /> itemsFormatted.<span class="me1">push</span><span class="br0">&#40;</span><span class="br0">&#123;</span><br /> Name1<span class="sy0">:</span> item.<span class="me1">Value1</span><span class="sy0">,</span><br /> Name2<span class="sy0">:</span> item.<span class="me1">Value2</span><span class="sy0">,</span><br /> Name3<span class="sy0">:</span> item.<span class="me1">Value3</span><br /> <span class="br0">&#125;</span><span class="br0">&#41;</span><span class="sy0">;</span><br /> <span class="br0">&#125;</span><span class="br0">&#41;</span><span class="sy0">;</span><br /> <br /> <span class="kw1">var</span> fileTitle <span class="sy0">=</span> <span class="st0">'AdditionalCompanyContacts'</span><span class="sy0">;</span><br /> <br /> exportCSVFile<span class="br0">&#40;</span>headers<span class="sy0">,</span> itemsFormatted<span class="sy0">,</span> fileTitle<span class="br0">&#41;</span><span class="sy0">;</span><br /> <br /> <span class="br0">&#125;</span><span class="br0">&#41;</span><br /> .<span class="kw1">catch</span><span class="br0">&#40;</span>err <span class="sy0">=></span> <span class="br0">&#123;</span> <span class="kw1">throw</span> err <span class="br0">&#125;</span><span class="br0">&#41;</span><span class="sy0">;</span><br /> <span class="br0">&#125;</span><br /> <span class="sy0"></</span>script<span class="sy0">></span><br /> <br /> <span class="sy0"><</span>style type<span class="sy0">=</span><span class="st0">"text/css"</span><span class="sy0">></span><br /> .<span class="me1">download</span><span class="sy0">-</span>wrapper <span class="br0">&#123;</span><br /> cursor<span class="sy0">:</span> pointer<span class="sy0">;</span><br /> <span class="sy0">&:</span>hover <span class="br0">&#123;</span><br /> opacity<span class="sy0">:</span> .7<span class="sy0">;</span><br /> <span class="br0">&#125;</span><br /> <span class="br0">&#125;</span><br /> <span class="sy0"></</span>style<span class="sy0">></span>
        </div>
      </td>
    </tr>
  </table>
</div>

Then all you need to do is add a DIV that calls the download() function:

<div class="codecolorer-container javascript default" style="overflow:auto;white-space:nowrap;width:95%;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />
        </div>
      </td>
      
      <td>
        <div class="javascript codecolorer">
          <span class="sy0"><</span>div style<span class="sy0">=</span><span class="st0">"float: right;"</span> <span class="kw5">class</span><span class="sy0">=</span><span class="st0">"download-wrapper"</span> onClick<span class="sy0">=</span><span class="st0">"download()"</span><span class="sy0">></span><br /> <span class="sy0"><</span>i <span class="kw5">class</span><span class="sy0">=</span><span class="st0">"fa fa-download fa-lg"</span><span class="sy0">></</span>i<span class="sy0">></span> Export Data <span class="br0">&#40;</span>CSV<span class="br0">&#41;</span><br /> <span class="sy0"></</span>div<span class="sy0">></span>
        </div>
      </td>
    </tr>
  </table>
</div>

Again, the thanks to Danny Pule for the first part about how to client-side export.  Make sure to check out Danny&#8217;s blog for all sort of cool code tips.