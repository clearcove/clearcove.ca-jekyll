---
title: The Benefits of Reification in Software Engineering
author: jhund
layout: post
permalink: /2012/02/the-benefits-of-reification-in-software-engineering/
categories:
  - Software
---
Reification sounds horribly abstract, however it is an incredibly important concept, related to writing better software:

> By means of reification, something that was previously implicit, unexpressed, and possibly inexpressible is explicitly formulated and made available to conceptual (logical or computational) manipulation. Informally, reification is often referred to as &#8220;making something a first-class citizen&#8221; within the scope of a particular system.

Link: [Reification (computer science) &#8211; Wikipedia, the free encyclopedia][1]

As soon as you reify a concept in your code (typically by defining a method or data structure, or class), the concept has a name and a single location in your code. This gives you the following benefits:

  * you can refer to the concept using its name as a symbol. This makes for effective communications with colleagues.
  * you can test its behavior because it now has a clearly defined interface.
  * your code becomes adaptable since it&#8217;s easy to find all places in your code that refer to the concept via simple search, and the implementation of the concept is located in exactly one place.
  * it encourages the use of the least evil of all couplings in sofware: a concept known and reified as &#8220;[Connascence of Name][2]&#8220;.

<div>
</div>

 [1]: http://bit.ly/yAbiPs
 [2]: http://en.wikipedia.org/wiki/Connascent_software_components#Types_of_connascence