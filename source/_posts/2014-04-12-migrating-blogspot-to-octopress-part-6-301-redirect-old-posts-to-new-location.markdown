---
layout: post
title: "Migrating from BlogSpot to Octopress - Part 6 - "
date: 2014-04-12 20:19:53 -0700
comments: true
categories: 
  - Octopress
published: false
series_about: "Migrating from BlogSpot to Octopress"
series_title: "301 Redirect Old Posts to New Location"
series: migrate-blogspot-to-octopress
---

{% include series.html %}

So far we have mostly delt with getting the old blog content into our new Octopress blog. In this post I'll talk about how I setup automatic redirection from the old blog to the new blog.

> Sorry, this post gets a little long, mostly because there is lots evolved, and I took some time to explain it along the way.

So, I'm not an SEO expert or even novice, and I hope the steps I took below actually gave me my end goal. I *think* they did, but let's not get ahead of ourselves. If you have any feedback, please drop me a line in the comments or in a [github issue for this blog](https://github.com/staxmanade/staxmanade.github.io/issues).

### Reasons for redirection:

- I'd like people to arrive at my new site even if they followed a link to the old BlogSpot location and get all the benefits of the new site (new theme, broken links fixed, etc)
- The whole reason [I did this](/blog/2014/04/migrating-blogspot-to-octopress-part-1-introduction) was to 'migrate' not, start fresh.

I probably won't ever `delete` my `staxmanade.blogspot.com` blog due to outside blogs, articles and sites that have linked to posts I've made in the past, but at least they will end up at my new blog.

### Goals for the redirection.

- Users who click on a link to the old blog should redirect successfully to the new site.
- I would love it if Google and other search engines could follow a [301 Moved Permanently](http://en.wikipedia.org/wiki/HTTP_301) redirection with the hopes that my old blog's search rankings would carry over to my new blog.

    > DISCLAIMER: Given the steps in the post, I'm not 100% confident that this goal was accomplished, but I think it may have worked (maybe, possibly, ???)

Now, while I'm not 100% confident my search rankings carried over, I'm fairly confident that Google found out about my new blog because after just 2 days of the below steps being implemented, Google had already indexed the new site.


# What's involved for redirection?

1. Tell BlogSpot to redirect each post at `staxmanade.blogspot.com` to the new location.
2. Handle that redirection on the Octopress side.

There are a number of posts out there that describe how to accomplish this with WordPress or other blogs that can host a dynamic server side component. However with Octopress or other statically generated sites we don't have as easy a time. I belive this has more to do with the lack of programmability on the BlogSpot side (or my lack of knowledge of how far you can go with it on their side) than the static-ness of our Octopress blog.

### Let's get into why this wasn't as straight forward.

If you look at how to redirect from a [BlogSpot blog to say WordPress](http://www.shoutmeloud.com/how-to-migrate-from-blogspot-to-wordpress-with-301-permanent-redirection-without-loosing-traffic.html) you'll see that they are basically passing the FULL original url EX:`http://staxmanade.blogspot.com/2013/12/format-your-net-exceptions-to-see.html` to a server-side component, where it can dynamically translate that into the correct 301 redirect response needed for each page.

The problem we have is the [BlogSpot template tags](https://support.google.com/blogger/answer/42095?hl=en) are limited. They don't provide a *relative blog url* parameter which would have made it easy. Instead they provide only the full URL `<$BlogItemURL$>` which we can't use on our static site.

The direction I took was to use the `<$BlogItemNumber$>` which is like a really long `id` value, leverage the [Jekyll alias generator](https://github.com/tsmango/jekyll_alias_generator) and end up doing two redirects.

1. from `staxmanade.blogspot.com/SomeBlogUrl` -> `staxmanade.github.io/blog/<$BlogItemNumber$>`
2. then from `staxmanade.github.io/blog/<$BlogItemNumber$>` -> `staxmanade.github.io/blog/finalBlogUrl`

On the Octopress side we are going to leverage the [alias](https://github.com/tsmango/jekyll_alias_generator) plugin. Once that's installed, we need to update all of our blog posts to add in the 'alias' that will point to th e`/blog/{really_long_id}. My scripting hammer is `PowerShell` *(sorry if you're reading this on a mac - hey, maybe [Pash](https://github.com/Pash-Project/Pash) can help you if you're not on Windows)* and I wrote the below script to help in the task. Even if you don't know `PowerShell`, take a moment to read the comments in the code to get an idea of what it is doing.

{% gist 10562366 InsertBloggerAliasIntoJekyllPost.ps1 %} 

In summary, the above code is:

1. Parsing the blogger xml
2. Extracting the post url (slug) and BlogSpot blog `id`
3. Matching each post with a post in our Octopress `_posts` [that we recently exported](/2014/04/migrating-blogspot-to-octopress-part-3-export-blogspot-content)
4. And injecting an `alias: /blog/{id}` into the header `YAML`

Look in your blogger xml for a sample ID, it looks like

    <id>tag:blogger.com,1999:blog-4726251688144615011.post-7515836382847059837</id>

What we care about is the `BigNumber` after the "...post-<BigNumber>". This is the `id` used in the above script to generate our new `/blog/<BitNumber>` [alias](https://github.com/tsmango/jekyll_alias_generator) url.

If you completed the above and run `rake generate` and `rake preview`, test out that your redirections are correct.

After I completed this and on my blog, [staxmanade.com/blog/7515836382847059837](https://staxmanade.com/blog/7515836382847059837) should redirect me to an actual post.

Once you're happy with your Octopress's blogger redirections, **SHIP IT**. Publish them to your blog and test them out on gihub `rake gen_deploy`.

Now that we have our static site redirecting the special `/blog/{id}` url's, we can go back to BlogSpot and update our template to redirect to our static site.

I used the below template (note search/replace your domain etc) before dropping this into your site.

# Update Blogger template to redirect.

1. Go back to your BlogSpot admin page and select the `Template` tab.
2. On the lower right you may have to select (forgetting the exact text - but something like) `Revert to classic template`
3. Modify the below template for your own site.

{% gist 10562366 BloggerRedirect.html %} 

This will change all pages/posts on your old BlogSpot site (even your home page). You can now go test it out - see that it hopefully transitions you from your blogger site -> to the new static redirect - and then to your final post location. It happens so fast I sometimes don't see the middle/transition page.

> Note: I had to run through this a few times before I got it right so be careful doing this (especially if you have a popular blog as it may interrupt users at the current time). I'm not that cool, so my blog could handle some downtime :P
