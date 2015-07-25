---
title: 'Keynote: Architecture the Lost Years &#8211; Robert Martin &#8211; Ruby Midwest 2011'
author: jhund
layout: post
permalink: /2012/03/keynote-architecture-the-lost-years-robert-martin-ruby-midwest-2011/
categories:
  - Blog
  - Curated News
---
Bob Martin at 43:30:

> That&#8217;s what architecture is: Find some place to draw a line, and then make sure every dependency that crosses that line goes in the same direction.

He reminds us of an architectural pattern that was presented in the early nineties by Ivar Jacobson:

  * Boundary interfaces: Interfaces that define how a control object interacts with an actor.
  * Entity objects: They are business specific, however not app specific. E.g., an Order, a Document, a Product. They don&#8217;t care about persistence. Just the data structures and business rules.
  * Control objects: Also referred to as Interactors, one for each Use Case. They are business and app specific. They integrate Actors, Entities other control objects, and supporting components.

Link: [Keynote: Architecture the Lost Years &#8211; Robert Martin &#8211; Ruby Midwest 2011][1] via confreaks.com

 [1]: http://bit.ly/FOFHym