---
title: 'Random Ruby Tricks: Struct.new &#8211; Literate Programming'
author: jhund
layout: post
permalink: /2012/09/random-ruby-tricks-struct-new-literate-programming/
categories:
  - Blog
  - Curated News
---
Neat trick about using Ruby Struct for simple domain concepts:

<pre>Point = Struct.new(:x, :y)
origin = Point(0,0)</pre>

Struct.new returns a class, which we assign to the Point constant. Very cool. We just created a simple class with constructor and two accessors (x and y).

Link: [Random Ruby Tricks: Struct.new &#8211; Literate Programming][1] via blog.steveklabnik.com

 [1]: http://bit.ly/OMnj8C