---
layout: post
title: "Going low-tech: Blogging with jekyll"
category: life
---

Most of the groundwork is done, therefore I can finally explain what happend here:

I started my blog on [blogger.com][blogger] because it seemed like a quick and convenient way to publish my occasional article about GNU/Linux, life and technology, but over time I got a bit annoyed with it. The system generates the current view dynamically with heavy use of javascript which leads to parts of the page breaking frequently if one of the content delivery servers doesn't respond in time. On my blog, prime candidates for this kind of breakage were the top and the slide-in navigation. A blog with most of the navigation missing? No, thanks. In addition to that I had no control over the source code and the rich text editor behaves nothing like my usual tools of the trade. Those were itches I definitely wanted to be scratched.

The solution: Running my own blog somewhere safe.

<!--more-->

## Quest for an alternative
My top three requirements were as follows:

1. Small technology footprint
2. Access to the source code
3. Comfortable way to write posts

Due to my prior knowledge my choices basically boiled down to three options:

1. Wordpress
2. Writing my own blog engine with [play!][play] or [ruby on rails][rails]
3. Digging into the world of static blog generators

I don't want to develop in php, therefore Wordpress was off the table. Writing my own blog engine seemed a bit overkill and to be honest: *no one* needs *another* blog engine. Ergo the only valid choice was to look for a suitable static blog generator. Of course many blog engines not written in php exist, but static generation has a few convincing advantages.

After short research my decision was between [Blogofile][blogofile] and [Jekyll][jekyll] and with the latter written in ruby and the option to be hosted on [github pages][githubpages] I chose Jekyll.

## Low Tech, High Life
A blog usually consists of a content database, a bunch of CSS templates for the frontend, some backend-voodoo in between and a bunch of javascript to glue everything neatly together. In return you get a lot of nice features like comments, rich-text-editing, drag-and-drop rearranging, etc.

Jekyll as a static blog generator works different. It too has templates in terms of [Liquid][liquid] infused HTML and is able to include javascript, but it offers a complete lack of a database and server-side business logic. This opens up several advantages:

- No ugly user input that has to be dealt with
- Posts are written in your text editor of choice
- No server-side code means less bugs
- Static files scale incredibly well

Posts are written in plain text using Markdown and Jekyll generates static files for the page. Subsequently the files can be uploaded to the web-server of choice. Done. This leads to very appealing workflows:

Currently I'm writing this post in VIM and reviewing the final look in a browser window pointing to localhost. Afterwards I'll commit the post to a git repository and push it onto github which in turn will compile the post and add the static files to my blog. **BAM! Can't beat that workflow, Wordpress!**

And this explains why my blog now lacks comments, javascript and breakage. Comments are possible via mail or social networks, javascript is not needed and if you miss the breakage, you might be a terrible person.

##I want my own!
To get your own glorious Jekyll blog, have a look at its [documentation][jekylldoc]. The Jekyll website is itself powered by Jekyll and the source code is visible on [github][jekyllgithub] for further inspiration. Maybe I'll write a short tutorial for a nice setup some time in the future. Maybe.

[blogger]: http://suddenkernelpanic.blogspot.de
[blogofile]:http://www.blogofile.com/ 
[githubpages]: https://pages.github.com
[jekyll]: http://www.jekyllrb.com
[jekylldoc]: http://jekyllrb.com/docs/home/ 
[jekyllgithub]: https://github.com/mojombo/jekyll/tree/master/site
[liquid]: http://wiki.shopify.com/Liquid 
[play]: http://www.playframework.com
[rails]: http://www.rubyonrails.org
