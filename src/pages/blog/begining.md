---
setup: |
  import Layout from '../../layouts/BlogPostLayout.astro';
title: New begining
description: Creating a blog using static site generator
date: 2019-11-30
author: "Gold Ayan"
category: "Coding"
tags:
  - Zola
  - SSG
---

Previously i used **blogspot** to manage blog. I thought for every long time that i
need to host my own website using static site generator which will be easy to
maintain and i can customize almost anything in the website.

key functionality that i need

- Full customize ability
- Simple software stack ( so i can change it if i need something )
- Able to change software stack.
<!-- more -->

I am not posting any new article in my blogspot for quite a long time now.
Searching lot of software finally choose to continue my blogging journey using
[**Zola**](https://www.getzola.org) static site generator which is written in
**Rust** and it is opensource.

Reason for choosing Zola:

- It is fast.
- It is written in **Rust** programming language(I like Rust).

what can you expect from this blog

- Technology i mess with
- Problems that i face in programming and how i solved
- CTF writeup

It is not only limited to above, i will try to write article out of scope if
topics are interesting. I want to encourage others to write blog about things
they passionate about.

There are rooms to improve blog apperance, i am in bit hurry to deploy it as fast as possible.
so i want to concentrate on article as far now. once i have free time i will try
to make blog apperance much more polish and add few functionality.

### Writing article

Articles are written in **markdown**.Can use any editor to write and we can also
use mobile phones. Markdown syntax are simple and easy to learn.more info on
markdown click [here](https://www.markdownguide.org)

### Known issues [Fixed]

Currently search doesnot work. I donot know why the search index are not generated
properly.working on it will fix it soon.

- I forget to provide **in_search_index = true** in page front matter.
- I works now :)

### Comments

You have few problems when making your blog with static site generator among
those most important for me is **comments**.

You need to put extra effort in configuring comment section in static site
generator. i was planned to use [**disqus**](https://disqus.com) it is great
but for free tier they display ads. I don't want any ads in my site so i was
searching for alternatives and i find out [**utterances**](https://utteranc.es)
which uses github issue's as comments which is pretty cool :).But it has one
downside though you need to have github account to post comment. I decided to
choose this because most of the article going to target developers, so they
have a github account and I encourage others to create account in github.It can
be used for other purpose too such maintaining notes and other stuff.

### Storage

I used github to store my blog whenever i want to update my blog, just pushing
a new changes to particular repository will update the blog but now you may ask
how you are going to host the blog. There is option to host the respository as
blog in github but i used other service which is Netlify.

### Hosting

I used [Netlify](https://www.netlify.com) to host my blog.Netlify can be configured
to run the build command automatically when i push new changes to github. so you
donot need to manually update the blog.

That's it...

This blog uses **Zulma** theme which is available in
[here](https://www.getzola.org/themes/zulma/). Thanks to Zola developers
for creating this software.

### Changelog

- **10-05-2020** : Serch bar issue fixed.
