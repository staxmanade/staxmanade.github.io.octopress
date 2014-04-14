---
layout: post
title: "Migrating from BlogSpot to Octopress - Part 5 - Fix Links"
date: 2014-04-12 20:19:53 -0700
comments: true
categories: 
  - Octopress
published: false
series_about: "Migrating from BlogSpot to Octopress"
series_title: "Fix Links"
series: migrate-blogspot-to-octopress
---

{% include series.html %}

Once you've ported your content into Octopress there were several steps I used to fixup links. The two types of links I cared about at this stage were:

1. Cross-Post links (where I referenced one of my other posts)
2. Broken links.

## Cross post links

I used a text editor and some command line magic to search for `http://staxmanade.blogspot.com/` and replace it with `/blog/` so that my cross-referencing posts could link to a relative version of the blog instead of the full blogspot domain.

Depending on how you configure your [permalinks](http://jekyllrb.com/docs/permalinks/) you may need to do some more link manipulation. I had to search `.html` at the end of my cross-referencign posts and be sure to delete it since my old reference would look like

    http://staxmanade.blogspot.com/2013/12/format-your-net-exceptions-to-see.html

but now should link to

    /blog/2013/12/format-your-net-exceptions-to-see

If you're on Windows and not interested in figuring out a `PowerShell` or other command to quickly search and replace, a friend of mine [Tim Greenfield](http://programmerpayback.com/about/) has a great utility GUI tool for easy [search and replace](https://seeker.codeplex.com).

I don't recall exactly what I did, I think I either used `sed` or a python command on my Mac for the initial search/replace. I'll let you figure out the rest of how to get that task done.

## Fix broken links

Once you're done fixing up cross-post links, we want to make sure we didn't mess anything up, and while we're at it, fix any old or out-dated links.

One great feature of [Octopress](http://octopress.org) is that we can run the site locally and use a spider tool to search for broken links.  Run `rake generate` and `rake preview` locally to browse your site.

I used the [Integrity link checker](http://peacockmedia.co.uk/integrity/) on my Mac to search the `http://localhost:4000` site locally. There are lots of these tools out there, so feel free to use what you feel happy with.

This was a great exercise. Not only debugging any *oopsies* from the above cross-post fixup step, but allowed me to find any external links to blogs/images/etc that were out of date. I wasn't able to fix up all of my external links, but that's the way of the web unfortunately. I haven't done it yet, but have consider going back and linking the out-dated links to a version out on the [Way Back Machine](http://web.archive.org/).
