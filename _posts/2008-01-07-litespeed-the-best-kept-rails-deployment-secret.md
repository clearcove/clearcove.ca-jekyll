---
title: 'Litespeed: The best kept Rails deployment secret'
author: jhund
layout: post
permalink: /2008/01/litespeed-the-best-kept-rails-deployment-secret/
categories:
  - Ruby/Rails
---
There is a great way to deploy rails applications. I know it is being used on some high traffic production deployments. How come it never gets mentioned in any official Rails documentation?

[Litespeed][1] is its name. It is free — not open source though. And that may be the reason why you don&#8217;t hear much about it in the Rails community.

<!--more-->

Why I like it?

  1. Super simple deployment. No mongrels, monit, or apache. Litespeed replaces all three of them: It has the front end, load balancer, and manages its own Ruby processes.
  2. Nice GUI for system management: server configuration, control, and updates are all handled through a super simple and sleek UI.
  3. It is free to use for most small to medium scale web apps. You pay for the &#8220;Enterprise&#8221; version.
  4. Small memory footprint. Gives you more apps per MB of RAM on your server. That translates directly into saved hosting dollars and makes for snappier apps.

usefuljaja has a bunch of [tutorials covering litespeed][2].

 [1]: http://www.litespeedtech.com/
 [2]: http://www.usefuljaja.com/litespeed