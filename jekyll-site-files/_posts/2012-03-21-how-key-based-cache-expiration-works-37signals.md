---
title: 'How key-based cache expiration works &#8211; (37signals)'
author: jhund
layout: post
permalink: /2012/03/how-key-based-cache-expiration-works-37signals/
categories:
  - Blog
  - Curated News
---
This is how 37Signals does key-based cache expiration:

> **The cache key is the fluid part and the cache content is the fixed part**. A given key should always return the same content. You never update the content after it&rsquo;s been written and you never try to expire it either.

Link: [How key-based cache expiration works &#8211; (37signals)][1] via 37signals.com

 [1]: http://bit.ly/FOScpO