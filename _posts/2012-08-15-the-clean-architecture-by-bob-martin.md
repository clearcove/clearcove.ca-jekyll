---
title: The Clean Architecture by Bob Martin
author: jhund
layout: post
permalink: /2012/08/the-clean-architecture-by-bob-martin/
categories:
  - Blog
  - Curated News
---
Bob Martin describes what a universal and clean architecture for software apps could look like. He describes the continuum of

  * concrete to abstract/high level
  * concentricity (immutable enterprise entities are in the center, then use cases, interface adapters and outermost the frameworks and drivers)

<div>
  A key element is the &#8220;Dependency Rule&#8221;:
</div>

> This rule says that source code dependencies can only point inwards. Nothing in an inner circle can know anything at all about something in an outer circle. In particular, the name of something declared in an outer circle must not be mentioned by the code in the an inner circle. That includes, functions, classes. variables, or any other named software entity.

I wonder how I can marry these ideas with Ruby on Rails&#8230;

Link: [The Clean Architecture | 8th Light][1] via blog.8thlight.com

 [1]: http://bit.ly/PW0j6c