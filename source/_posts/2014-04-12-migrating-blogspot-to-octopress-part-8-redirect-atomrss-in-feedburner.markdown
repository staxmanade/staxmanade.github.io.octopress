---
layout: post
title: "Migrating from BlogSpot to Octopress - Part 8 - Redirect Atom/RSS in FeedBurner"
date: 2014-04-12 20:19:53 -0700
comments: true
categories:
  - Octopress
published: false
series_about: "Migrating from BlogSpot to Octopress"
series_title: "Redirect Atom/RSS in FeedBurner"
series: migrate-blogspot-to-octopress
---

{% include series.html %}

This one is quick and easy.

I noticed that my original RSS feed `staxmanade.blogspot.com/feeds/posts/default` was 302 redirecting to `http://feeds.feedburner.com/DevelopingOnStaxmande`.

So I went to `feedburner.com` where I was already logged in (due to being auth'd with google/blogger) and from their I selected my feed and chose `Edit Feed Details` where I could put in my new RSS feed to `http://staxmanade.com/atom.xml`.

After doing this, I noticed 3 of my previously posted blogs show-up-again, but I can live with that.

Now you've hopefully migrated your blog's RSS subscribers automatically without loosing any.
