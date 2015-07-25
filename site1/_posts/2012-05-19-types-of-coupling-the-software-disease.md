---
title: Types of Coupling (the software disease)
author: jhund
layout: post
permalink: /2012/05/types-of-coupling-the-software-disease/
categories:
  - Blog
  - Curated News
---
The article examines different types of coupling and what they look like in Rails:

> Coupling refers to the degree to which components in your program rely on each other. You should generally seek to minimize this property, though you&rsquo;ll see it&rsquo;s impossible to eliminate entirely.

  * Pathological coupling: one class reaches into another and changes instance variables.
  * Global coupling: Two classes that rely on some global state.
  * Control coupling: a parameter is passed to a method that determines the control flows in the method.
  * Data coupling: a parameter is passed to a method that does not impact the control flow in the method.
  * Message coupling: Jim Weirich calls this &#8220;Connascence by Name&#8221;. The least costly type of all couplings. This is the one we should strive for.

Link: [Types of Coupling][1] via robots.thoughtbot.com

 [1]: http://bit.ly/JEIbyv