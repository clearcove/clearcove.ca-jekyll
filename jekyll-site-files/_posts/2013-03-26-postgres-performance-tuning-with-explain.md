---
title: Postgres performance tuning with EXPLAIN
author: jhund
layout: post
permalink: /2013/03/postgres-performance-tuning-with-explain/
categories:
  - Note to self
  - Tools
---
Here are some tips for tuning postgres performance using the EXPLAIN command:

  * Use &#8220;EXPLAIN (ANALYZE, BUFFERS) SELECT &#8230;&#8221; as of PG 9.2.
  * Paste the output into this service to highlight areas of concern: <http://explain.depesz.com/>
  * Learn about EXPLAIN from the detailed documentation: <http://www.postgresql.org/docs/current/interactive/using-explain.html>
  * This Wiki page has more pointers to other tools and tutorials around EXPLAIN: <http://wiki.postgresql.org/wiki/Using_EXPLAIN>