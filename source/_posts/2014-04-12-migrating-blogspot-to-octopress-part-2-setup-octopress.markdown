---
layout: post
title: "Migrating from BlogSpot to Octopress - Part 2 - Setup Octopress"
date: 2014-04-12 20:19:53 -0700
comments: true
categories: 
  - Octopress
published: false
series_about: "Migrating from BlogSpot to Octopress"
series_title: "Setup Octopress"
series: migrate-blogspot-to-octopress
---

{% include series.html %}

Luckily for all of us, this post will be short as the [Octopress Setup](http://octopress.org/docs/setup/) guide is a great place to start. Only additions I have to add are that I've set this up on both a Mac and a Windows 8 machine.

I had to execute the `rake setup_github_pages[repo]` on both machines. I should probably [look under the abstraction](http://www.hanselman.com/blog/PleaseLearnToThinkAboutAbstractions.aspx) and understand a little deeper - but I'll have to save that for later. For now I have work to do...

### On my Mac:

- Things worked out quite smoothly on the Mac.
- This is where I ran my initial import of the blog (see later posts in the series).

### On my Windows 8 machine:

- This environment seemed harder to get going
  - The hardest part on Windows 8 was getting the right combination of ruby, ruby dev kit, and various other dependencies installed. Since I don't use Ruby on Windows for anything else, I eventually ended up uninstalling all versions I'd previously had (including cleaning up any environment path variables). Re-installing just what I needed and eventually got it working.
- [Thanks to this post](http://blog.zerosharp.com/setting-up-octopress-on-windows/) to get started on the windows. Some steps I excluded since I'd alredy setup the blog initially on my Mac.

Now I can use the power of git to manage my website and blog and I can leverage whatever development tools I would like depending on the platform.
