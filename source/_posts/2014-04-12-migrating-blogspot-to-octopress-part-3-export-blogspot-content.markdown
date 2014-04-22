---
layout: post
title: "Migrating from BlogSpot to Octopress - Part 3 - Export BlogSpot Content"
date: 2014-04-12 20:19:53 -0700
comments: true
categories: 
  - Octopress
published: false
series_about: "Migrating from BlogSpot to Octopress"
series_title: "Export BlogSpot Content"
series: migrate-blogspot-to-octopress
---

{% include series.html %}

Thanks to Google for allowing us to [export our own content](https://support.google.com/blogger/answer/97416?hl=en) and providing an easy way to get it all in one download. If we had to go manually read each item in an atom feed or web scrape the content off of a web page, this small series would be more like a novel...

# Steps to export content.

1. Log in to your [BlogSpot.com(http://BlogSpot.com) admin portal
2. Go to the `Settings` tab
3. Select the `Other` option under Settings
3. Under Blog tools select `Export blog`
4. Save the file somewhere safe so we can refer to it later

![Blogger Settings](/images/posts/blogger_settings.png)

## What's in this export file?

This file contains the raw content and metadata around each of your blog posts. You won't get any of your images, but all of your original blog images are probably hosted on external sites. This is next on my list of things to add to the port-list (and will possibly extend this series even more). I would like to download each of my images locally, save them to my Octopress [git repo](http://github.com/staxmanade/staxmanade.github.io) and update my posts to link to the local versions of the images (but that's lower on the priority).

Here is a snippet of a single entry from the exported file. In later posts we'll delv deeper into this to extract metadata an setup redirection from BlogSpot to our new blog.

## Sample Entry from Blogspot Export File
```xml
  <entry>
    <id>tag:blogger.com,1999:blog-4726251688144615011.post-7515836382847059837</id>
    <published>2013-12-18T19:50:00.001-08:00</published>
    <updated>2013-12-18T19:50:18.725-08:00</updated>
    <category scheme='http://schemas.google.com/g/2005#kind' term='http://schemas.google.com/blogger/2008/kind#post'/>
    <title type='text'>Format your .Net exceptions to see the StackTrace.</title>
    <content type='html'>&lt;h4&gt;TL;DR&lt;/h4&gt;  &lt;p&gt;Check out a dinky little &lt;a href="http://staxmanade.github.io/ExceptionMessageBeautifier" target="_blank"&gt;Exception Message Beautifier&lt;/a&gt; site I threw together so you can quickly format .net exception messages and easily see the StackTrace.&lt;/p&gt;  &lt;p&gt;&amp;#160;&lt;/p&gt;  &lt;h4&gt;Go to the site: &lt;a href="http://staxmanade.github.io/ExceptionMessageBeautifier" target="_blank"&gt;&lt;font size="4"&gt;CLICK HERE&lt;/font&gt;&lt;/a&gt;&lt;/h4&gt;  &lt;p&gt;&amp;#160;&lt;/p&gt;  &lt;h4&gt;Background&lt;/h4&gt;  &lt;p&gt;Over the years, I’ve worked on projects where application exceptions were saved to a SQL database. When querying the logs in Visual Studio or in Sql Management Studio’s table view, I would get a result-set that would not let me copy/paste and review the StackTrace easily. The tool always seemed to leave out the new line characters just like below.&lt;/p&gt;  &lt;blockquote&gt;   &lt;pre&gt;System.Exception: Hello Exception!   at TestExceptionGenerator.Spike.GetException() in c:\Code\personal\DotNetExceptionMessageFormatter\TestExceptionGenerator\Spike.cs:line 22   at TestExceptionGenerator.Spike.b__0() in c:\Code\personal\DotNetExceptionMessageFormatter\TestExceptionGenerator\Spike.cs:line 13   at TestExceptionGenerator.Extensions.GetExceptionString(Action action) in c:\Code\personal\DotNetExceptionMessageFormatter\TestExceptionGenerator\Spike.cs:line 34&lt;/pre&gt;&lt;br /&gt;&lt;/blockquote&gt;&lt;br /&gt;&lt;br /&gt;&lt;p&gt;&amp;#160;&lt;/p&gt;&lt;br /&gt;&lt;br /&gt;&lt;p&gt;Now, I know there are ways to get around this, like exporting to CSV, or setting up the query results to return in text view instead of table view. However, when you’re in the heat of tracking down a bug and don’t feel like you have time to find the settings dialog or open up you’re a text editor like &lt;a href="http://notepad-plus-plus.org/" target="_blank"&gt;NotePad++&lt;/a&gt; and enter a search/replace as I show below over and over with each exception message you review.&lt;/p&gt;&lt;br /&gt;&lt;br /&gt;&lt;p&gt;&lt;a href="http://lh6.ggpht.com/-FGWJCcb9iUo/UrJs8rxtvqI/AAAAAAAAAik/uWK5NHFfU18/s1600-h/image15.png"&gt;&lt;img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; float: none; padding-top: 0px; padding-left: 0px; margin-left: auto; display: block; padding-right: 0px; border-top-width: 0px; margin-right: auto" border="0" alt="image" src="http://lh5.ggpht.com/-Eo5EyWOkIm0/UrJs9Dr20HI/AAAAAAAAAio/9-9wJIb1K1c/image_thumb9.png?imgmax=800" width="443" height="286" /&gt;&lt;/a&gt;&lt;/p&gt;&lt;br /&gt;&lt;br /&gt;&lt;p&gt;Just so I could see an exception that looked more like:&lt;/p&gt;&lt;br /&gt;&lt;br /&gt;&lt;blockquote&gt;&lt;br /&gt;  &lt;p&gt;System.Exception: Hello Exception! &lt;br /&gt;    &lt;br /&gt;&amp;#160;&amp;#160; at TestExceptionGenerator.Spike.GetException() in …&amp;lt;cut off for brevity&amp;gt; &lt;br /&gt;&lt;br /&gt;    &lt;br /&gt;&amp;#160;&amp;#160; at TestExceptionGenerator.Spike.b__0() in …&amp;lt;cut off for brevity&amp;gt; &lt;br /&gt;&lt;br /&gt;    &lt;br /&gt;&amp;#160;&amp;#160; at TestExceptionGenerator.Extensions.GetExceptionString(Action action) in …&amp;#160;&amp;#160; &lt;/p&gt;&lt;br /&gt;&lt;/blockquote&gt;&lt;br /&gt;&lt;br /&gt;&lt;p&gt;I finally buckled down and threw together a tool for this. &lt;/p&gt;&lt;br /&gt;&lt;br /&gt;&lt;h3&gt;You can check go check out &lt;a href="http://staxmanade.github.io/ExceptionMessageBeautifier" target="_blank"&gt;Exception Message Beautifier&lt;/a&gt; where you can see the sample below.&lt;/h3&gt;&lt;br /&gt;&lt;br /&gt;&lt;p&gt;&amp;#160;&lt;/p&gt;&lt;br /&gt;&lt;br /&gt;&lt;p&gt;&lt;a href="http://lh3.ggpht.com/-Ts_VJAZqmu4/UrJs9i36LfI/AAAAAAAAAiw/j4jZDVL2Z-Y/s1600-h/image3.png"&gt;&lt;img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; float: none; padding-top: 0px; padding-left: 0px; margin-left: auto; display: block; padding-right: 0px; border-top-width: 0px; margin-right: auto" border="0" alt="image" src="http://lh6.ggpht.com/-YwT0ZJIBFSA/UrJs93yI9aI/AAAAAAAAAi4/eg2rZr285QE/image_thumb1.png?imgmax=800" width="689" height="354" /&gt;&lt;/a&gt;&lt;/p&gt;&lt;br /&gt;&lt;br /&gt;&lt;p&gt;&amp;#160;&lt;/p&gt;&lt;br /&gt;&lt;br /&gt;&lt;h4&gt;I’d like to thank.&lt;/h4&gt;&lt;br /&gt;&lt;br /&gt;&lt;p&gt;Below are a list of tools/resources I leveraged to put the site together relatively quickly over the weekend.&lt;/p&gt;&lt;br /&gt;&lt;br /&gt;&lt;ul&gt;&lt;br /&gt;  &lt;li&gt;&lt;a href="http://codepen.io/" target="_blank"&gt;CodePen.io&lt;/a&gt; where I first prototyped/built my site before porting it into the GitHub pages. &lt;/li&gt;&lt;br /&gt;&lt;br /&gt;  &lt;li&gt;&lt;a href="https://github.com" target="_blank"&gt;GitHub&lt;/a&gt; for providing us with &lt;a href="http://pages.github.com/" target="_blank"&gt;GitHub Pages&lt;/a&gt;. Made this site a piece of cake to setup. &lt;/li&gt;&lt;br /&gt;&lt;br /&gt;  &lt;li&gt;&lt;a href="http://github.com/approvals/Approvals.NodeJS" target="_blank"&gt;Approvals.NodeJS&lt;/a&gt; – easily test/verify output. (&lt;em&gt;Disclaimer – I created this nodejs port of &lt;a href="http://approvaltests.sourceforge.net/" target="_blank"&gt;Approvals&lt;/a&gt; for fun a while back and didn’t get around to throwing some polish on the library till now, where I was able to &lt;a href="http://en.wikipedia.org/wiki/Eating_your_own_dog_food" target="_blank"&gt;dog-food&lt;/a&gt; it&lt;/em&gt;) &lt;img class="wlEmoticon wlEmoticon-smile" style="border-top-style: none; border-bottom-style: none; border-right-style: none; border-left-style: none" alt="Smile" src="http://lh4.ggpht.com/-bZZmkqr5Fqc/UrJs-TZgV2I/AAAAAAAAAjA/22oydAVrbSM/wlEmoticon-smile2.png?imgmax=800" /&gt; &lt;/li&gt;&lt;br /&gt;&lt;br /&gt;  &lt;li&gt;&lt;a href="http://angularjs.org/" target="_blank"&gt;AngularJS&lt;/a&gt; (a bit overkill for this site, OK TOTAL OVERKILL, but was simple, easy, makes my JS very little, and will allow for easy growth down the road if it needs to.) &lt;/li&gt;&lt;br /&gt;&lt;br /&gt;  &lt;li&gt;&lt;a href="https://developers.google.com/speed/libraries/devguide" target="_blank"&gt;Google CDN&lt;/a&gt; for hosting AngularJS &lt;/li&gt;&lt;br /&gt;&lt;br /&gt;  &lt;li&gt;&lt;a href="http://google.com/analytics/" target="_blank"&gt;Google Analytics&lt;/a&gt; (so I can see if anyone cares) &lt;/li&gt;&lt;br /&gt;&lt;/ul&gt;&lt;br /&gt;&lt;br /&gt;&lt;ul&gt;If you take a look, find a bug. Submit a GitHub issue and/or a pull request. Or if you find it useful, feel free to let me know.&lt;/ul&gt;&lt;br /&gt;&lt;br /&gt;&lt;ul&gt;Enjoy!&lt;/ul&gt;  </content>
    <link rel='replies' type='application/atom+xml' href='http://staxmanade.blogspot.com/feeds/7515836382847059837/comments/default' title='Post Comments'/>
    <link rel='replies' type='text/html' href='https://www.blogger.com/comment.g?blogID=4726251688144615011&amp;postID=7515836382847059837' title='0 Comments'/>
    <link rel='edit' type='application/atom+xml' href='https://www.blogger.com/feeds/4726251688144615011/posts/default/7515836382847059837'/>
    <link rel='self' type='application/atom+xml' href='https://www.blogger.com/feeds/4726251688144615011/posts/default/7515836382847059837'/>
    <link rel='alternate' type='text/html' href='http://staxmanade.blogspot.com/2013/12/format-your-net-exceptions-to-see.html' title='Format your .Net exceptions to see the StackTrace.'/>
    <author>
      <name>Jason Jarrett</name>
      <uri>https://plus.google.com/112910204617314300568</uri>
      <email>noreply@blogger.com</email>
      <gd:image rel='http://schemas.google.com/g/2005#thumbnail' width='32' height='32' src='//lh6.googleusercontent.com/-Lz16PAsLf5Q/AAAAAAAAAAI/AAAAAAAAAg8/FYSu9U-1tCw/s512-c/photo.jpg'/>
    </author>
    <media:thumbnail xmlns:media='http://search.yahoo.com/mrss/' url='http://lh5.ggpht.com/-Eo5EyWOkIm0/UrJs9Dr20HI/AAAAAAAAAio/9-9wJIb1K1c/s72-c/image_thumb9.png?imgmax=800' height='72' width='72'/>
    <thr:total>0</thr:total>
  </entry>
```
