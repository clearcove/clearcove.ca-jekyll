---
title: 'CranialPulse: Specify What, Not How'
author: jhund
layout: post
permalink: /2012/08/cranialpulse-specify-what-not-how/
categories:
  - Blog
  - Curated News
---
Some good thoughts about how to name and write specs in Ruby/RSpec. However I agree that there seems to be a bit too much abstraction/duplication and prefer the first commenter&#8217;s approach:

> I worry about duplicating equality comparisons, so I&#8217;d probably do this instead, in each spec: convert(1).should == &#8216;I&#8217;; convert(2).should == &#8216;II&#8217;, and so on. Then I define #convert inside my context to invoke Calculator.convert(actual). Not quite as cute, but avoids reimplementing parts of should equal to, as your example has done.

Link: [CranialPulse: Specify What, Not How][1] via www.cranialpulse.com

 [1]: http://bit.ly/QfB13i