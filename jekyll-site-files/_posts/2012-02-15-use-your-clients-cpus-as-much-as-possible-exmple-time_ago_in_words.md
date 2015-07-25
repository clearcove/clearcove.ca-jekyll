---
title: 'Use your clients&#039; CPUs as much as possible, exmple: time_ago_in_words'
author: jhund
layout: post
permalink: /2012/02/use-your-clients-cpus-as-much-as-possible-exmple-time_ago_in_words/
categories:
  - Blog
  - Curated News
---
Servers are expensive. To save money, let&#8217;s use our clients&#8217; CPUs as much as possible. The additional cost to a client is negligible (e.g., an extra 50msec), however when you multiply that by 100,000 requests (approx. 1.4 hours of server time per day), it adds up.

I wonder which tasks could be outsourced to the client?

  * rendering templates (via client MVC and JSON API)
  * conversions (absolute time to relative time, time zones)
  * validations

Link: [Rails Best Practices | Not use time\_ago\_in_words][1]

 [1]: http://bit.ly/xdruVl