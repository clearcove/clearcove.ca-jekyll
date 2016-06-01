---
title: How to compare two directory trees via diff on OS X
author: jhund
layout: post
permalink: /2013/10/how-to-compare-two-directory-trees-via-diff-on-os-x/
categories:
  - Blog
---
This snippet will compare the two directory trees located at dir1 and dir2 and write the differences to a text file:

<blockquote class="iii-article-quote">
  <p>
    diff -qr dir1 dir2 | grep -v -e '.DS_Store' -e 'Thumbs' -e '.git' | sort > diffs.txt
  </p>
</blockquote>

<p class="iii-article-source">
  Link: <a href="http://hints.macworld.com/article.php?story=20070408062023352">Compare directories via diff &#8211; Mac OS X Hints</a> via hints.macworld.com
</p>
