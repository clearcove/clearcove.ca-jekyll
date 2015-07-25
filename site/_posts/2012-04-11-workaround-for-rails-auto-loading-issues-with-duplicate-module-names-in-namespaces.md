---
title: Workaround for Rails auto loading issues with duplicate module names in namespaces
author: jhund
layout: post
permalink: /2012/04/workaround-for-rails-auto-loading-issues-with-duplicate-module-names-in-namespaces/
categories:
  - Blog
  - Curated News
---
A workaround for Rails class loading issues where a namespaced module has the same name as a class:

> How to deal with load\_missing\_constant ambiguous behaviour?  
> It happens when you name your nested models like classes or modules being visible in global scope, consider User::Ticket and Ticket. Rails somehow fallbacks to ::Ticket when including User::Ticket. I bypass this by writing require_relative &#8216;user/ticket&#8217; and include User::Ticket. I know, it is not perfect but good enough for now.

Link: [Separate your concerns][1] via sevos.github.com

 [1]: http://bit.ly/Hqsdp6