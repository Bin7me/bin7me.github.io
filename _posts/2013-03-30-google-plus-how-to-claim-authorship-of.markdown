---
layout: post
title: "Google Plus: How to claim authorship of your blog posts on blogger"
category: entropy
---
## Introduction

When using google for research purposes, you'll often come across search results having a little picture aside and linking to the google+ profile of the author. See the picture below for an example.

[![search result with link to google+ profile][mobiflip]][mobiflip]

The purpose is clear: Linking abstract information on the web to the people behind and making people with google+ profiles more prominent. But how can you get the badge for your blog? Well, there are [hundreds of guides][ownershipGuides] out there describing what to do and what to link where, but I believe most of them tend to be overcomplicated and not suited for google's own product: Blogger. Therefore I decided to write down the necessary steps to accomplish this feat.

<!--more-->

## Step 0: Link from the blog to your profile

You'd almost think having the same account for your blog and your google+ profile would be sufficient to display oneself as the author, but no, you have to do it manually. Don't be afraid, it's easy:

Open the HTML-view of your current blog template and add the following link somewhere inside the `<head>`-tags.

{% highlight html %}
<link href='https://plus.google.com/[your-google+-profile-id]?rel=author'/>
{% endhighlight %}

You can find your profile-id in the url to your profile. (Who'd have thought that?) For me, the link looks like this:

{% highlight html %}
<link href='https://plus.google.com/106482422835272164694?rel=author'/>
{% endhighlight %}

## Step 1: Link from your profile to the blog

Simply go to your google+ profile and add a link to your blog's main page to the 'Contributor to' section. The result should look something like this:

[![add your blog to the contributor to section][contributor]][contributor]

## Step 2: Check the result

Google provides us with the [structured data testing tool][structuredDataTestingTool] to check which information can be derived from our page. Just enter the adress of your blog and click 'Preview'. The result should look similar to the picture:

[![output of the structured data testing tool][richsnippets]][richsnippets]

Hooray! It works! In a few days/weeks the connection should show up in real world search results. That's it.

## Addendum
2013-08-11: Despite the positive results of the structured data testing tool, my ownership is still not visible in the search results. Either something is flawed or broken. Sad panda.

[mobiflip]: {{site.url}}/assets/img/google-plus-ownership/mobiflip.png "search result with link to google+ profile"
[ownershipGuides]: https://www.google.de/search?q=claim+authorship+blog
[contributor]: {{site.url}}/assets/img/google-plus-ownership/contributorTo.png "add your blog to the contributor to section"
[structuredDataTestingTool]: http://www.google.com/webmasters/tools/richsnippets
[richsnippets]: {{site.url}}/assets/img/google-plus-ownership/richsnippets.png "output of the structured data testing tool"
