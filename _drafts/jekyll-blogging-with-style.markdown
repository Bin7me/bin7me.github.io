---
layout: post
title: "Jekyll: Blogging with style!"
category: technology
---
## Introduction
As promised in my [post about switching to Jekyll][jekyllBlogging], here's a short guide for a simple setup. I'll try to cover the basics and show an alternative to the usual 'styling with bootstrap' approaches. Plugins are not part of this tutorial, too, because the goal is to make the page compatible to github pages.

<!--more-->

## Setup
If your have ruby installed, the setup boils down to four simple steps:

{% highlight bash %}
$ gem install jekyll
$ jekyll new myNewFancyBlog
$ cd myNewFancyBlog
$ jekyll serve --watch
{% endhighlight %}

Now you can point your browser to `localhost:4000` and should see your new page up and running. The `--watch` switch watches for changes and rebuilds the site automatically.

Your directory now contains the following files and folders:

```
css
_layouts
_posts
_config.yml
index.html
```

[jekyllBlogging]: {% post_url 2013-09-08-going-low-tech-blogging-with-jekyll %}
[jekylldoc]: http://jekyllrb.com/docs/home/
