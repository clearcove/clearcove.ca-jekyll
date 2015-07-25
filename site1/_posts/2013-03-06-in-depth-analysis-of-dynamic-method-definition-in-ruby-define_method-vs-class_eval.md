---
title: In depth analysis of dynamic method definition in Ruby (define_method vs class_eval)
author: jhund
layout: post
permalink: /2013/03/in-depth-analysis-of-dynamic-method-definition-in-ruby-define_method-vs-class_eval/
categories:
  - Blog
---
> <p class="iii-article-excerpt">
>   TL;DR: depending on your app, using define_method is faster on boot, consumes less memory, and probably doesn&rsquo;t signigicantly impact performance. Throughout the Rails code base, I typically see dynamic methods defined using class_eval. What I mean by &ldquo;dynamic methods&rdquo; is methods with names or bod&#8230;
> </p>

<p class="iii-article-source">
  Aaron Patterson has a very detailed comparison of two methods for dynamically defining methods. He looks at performance, memory consumption and other parameters. He recommends using define_method, however he points out that define_method creates a closure, so it should not hold references to large objects.
</p>

<p class="iii-article-source">
  Link: <a href="http://tenderlovemaking.com/2013/03/03/dynamic_method_definitions.html">Dynamic Method Definitions | Tenderlovemaking</a> via tenderlovemaking.com
</p>