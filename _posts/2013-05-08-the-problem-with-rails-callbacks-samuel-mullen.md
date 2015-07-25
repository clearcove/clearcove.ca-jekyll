---
title: 'The Problem with Rails Callbacks &#8211; Samuel Mullen'
author: jhund
layout: post
permalink: /2013/05/the-problem-with-rails-callbacks-samuel-mullen/
categories:
  - Blog
---
Great article about usage of AR callbacks. Boils it down to this rule:

<blockquote class="iii-article-quote">
  <p>
    Use a callback only when the logic refers to state internal to the object.
  </p>
</blockquote>

Otherwise you will feel the pain of bad coupling when testing and continuing to work with your app.

<p class="iii-article-source">
  Link: <a href="http://samuelmullen.com/2013/05/the-problem-with-rails-callbacks/">The Problem with Rails Callbacks &#8211; Samuel Mullen</a> via samuelmullen.com
</p>