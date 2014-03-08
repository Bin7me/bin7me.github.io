---
layout: post
title: "Lessons learned from a few days with slow internet"
category: technology
---
Last year in December I was stuck with slow mobile internet for a few days and when I say _slow_, I'm talking about a very wonky edge connection at best. This made me realize a simple fact: many website don't care about bandwidth. They might be _responsive_ or even have a dedicated mobile version, but they are still loading tons of javascript and including lots of images on their landing page. I don't care if you have the best content when I have to wait for minutes until your page is fully loaded from a _bazillion_ content delivery networks. And if one of those requests has a timeout, half of the page is missing. Feels like [Blogger][jekyllBlogging] all over again. In this times I glanced at my blog and saw some room for improvement. Things had to change.

<!--more-->

[jekyllBlogging]: {% post_url 2013-09-08-going-low-tech-blogging-with-jekyll %}
