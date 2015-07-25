---
title: 'Require Ruby &#039;Forwardable&#039; on Leopard'
author: jhund
layout: post
permalink: /2008/05/require-ruby-forwardable-on-leopard/
categories:
  - Note to self
  - Ruby/Rails
---
When I upgraded to Leopard, some of my Rails apps broke. Ruby threw this error at me:

<pre>uninitialized constant User::Forwardable</pre>

Here is a simple fix:

<pre>require 'forwardable'
</pre>

in environment.rb.

It seems that on Tiger &#8216;Forwardable&#8217; was included by default, but not on Leopard any more. Not exactly sure why this is.