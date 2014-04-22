---
layout: post
title: "Migrating from BlogSpot to Octopress - Part 4 - Import Content into Octopress"
date: 2014-04-12 20:19:53 -0700
comments: true
categories: 
  - Octopress
published: false
series_about: "Migrating from BlogSpot to Octopress"
series_title: "Import Content into Octopress"
series: migrate-blogspot-to-octopress
---

{% include series.html %}


In the [previous post](/blog/2014/04/migrating-blogspot-to-octopress-part-2-setup-octopress) I discussed how to export your blog content from BlogSpot. Now that you have all of your content into a single `.xml` file, we need to translate that into the files and format that Octopress blog expects.

# Translate export `xml` to Octopress posts

Thanks to a ruby script found in a [gist](https://gist.github.com/juniorz/1564581), it's fairly easy to get going provided you are able to leverage all the gem dependencies with your installation of ruby. On Windows this is tricky although fortunately there are tools like [yari](https://github.com/scottmuc/yari) to help out.

1. Download and save one of these [gists](https://gist.github.com/dnagir/1765496/forks) somewhere and call it `BloggerImporter.rb`
  - Note: there are a number of forks of the script - if one doesn't work, browse other changes and see if something fits your needs.
2. From the command line go to a temporary folder and execute

    `ruby {pathToImporterFile}/BloggerImporter.rb '{PathToBloggerExport}/blog-03-19-2014.xml'`
    
    When I tried it on my windows machine I received the following error. Sorry Windows folks, I don't have answers for every scenario here - I just jumped over to my Mac and tried again. I remember first trying this like a year ago and ran down a similar rat-hole trying to get this dependency to work which is probably why I didn't port it back then. I'm sure it's possible, but wasn't worth my time (since it works on my mac and was a one-time task).
    
    ```ruby
    C:/Ruby193/lib/ruby/site_ruby/1.9.1/rubygems/custom_require.rb:36:in `require': cannot load such file -- nokogiri (LoadE
    rror)
            from C:/Ruby193/lib/ruby/site_ruby/1.9.1/rubygems/custom_require.rb:36:in `require'
            from BloggerImporter.rb:2:in `<main>'
    ```

    (One possible issue noted by a colleague: the ruby script requires a gem, [Nokigiri](http://nokogiri.org/), which is not supported on Windows by Ruby 2 at the time of this writing. This can be circumvented by either reinstalling Ruby 1.9.3 or by leveraging a tool like yari which allows you to pick the version of Ruby you wish to leverage with a script - a helpful StackOverflow answer exists [here](http://stackoverflow.com/a/17318410/64).)
    
3. You should now you have your content.
    
    After you've run the `BloggerImporter.rb` command you should have at least two folders in your temporary folder.
    
    ```
    ~/code/temp> ls
    _drafts     _posts    BloggerImporter.rb
    ```
    
    I didn't end up caring about anything in my `_drafts` folder, but the `_posts` folder is full of gold. This contains all of your exported content now broken out into a separate file per post in the form of `{year}-{month}-{day}-{title}.html`.

4. Copy this content into the Octopress blog folder under `source/_posts/`
5. At this point you've successfully ported your content into your Octopress blog. Make sure you take tiny steps and `git commit...` the changes you care about along the way...
6. Use `rake generate` and `rake preview` to see if it worked. 
7. If your preview looks right use `rake deploy` to put your changes on github.

Hopefully it all worked out and you're looking at your new Octopress blog with your old content all there. If I recall when I went through this process, there was one HTML file that `Jekyll` had issues compiling. I probably just modified the post or deleted somethign that wasn't necessary (I can't recall anymore) but it was quick to work through.


> side note: When I initially tried getting [Disqus](http://disqus.com) to work after this and had trouble. **Turned out this was because in the YAML metadata in each post `comments: false`** :)

