---
title: Big O notation for algorithm complexity explained via phone book analogy
author: jhund
layout: post
permalink: /2013/05/big-o-notation-for-algorithm-complexity-explained-via-phone-book-analogy/
categories:
  - Blog
---
<p class="iii-article-excerpt">
  <span style="line-height: 1.5em;">This stackoverflow answer does a great job in illustrating Big O notation for algorithm complexity. The author has a use case related to phone books for each level of complexity:</span>
</p>

<li class="iii-article-excerpt">
  O(1) &#8211; Given the page number and the person&#8217;s name, find the person&#8217;s phone number.&nbsp;<span style="line-height: 1.5em;">[Comment by JH: I find this one weak because it assumes constant time for looking up a page&nbsp;</span><span style="line-height: 1.5em;">number, when in reality is&#8217;s more like log(n). A better example would be to&nbsp;</span><span style="line-height: 1.5em;">find a random person&#8217;s phone number by flipping open any page in the phone book].</span>
</li>
<li class="iii-article-excerpt">
  O(log n) &#8211; Given a person&#8217;s name, find the corresponding phone number (This is a binary&nbsp;<span style="line-height: 1.5em;">search for a person&#8217;s phone number).</span>
</li>
<li class="iii-article-excerpt">
  O(n) &#8211; Find all people whose phone numbers contain the digit &#8220;5&#8243;.
</li>
<li class="iii-article-excerpt">
  O(n log n) &#8211; Sort a phone book&#8217;s pages by looking at the first name on each page.
</li>

<p class="iii-article-excerpt">
  Read the original article for higher complexity examples:&nbsp;<span style="line-height: 1.5em;">&nbsp;</span><a style="line-height: 1.5em;" href="http://stackoverflow.com/questions/2307283/what-does-olog-n-mean-exactly/2307314">algorithm &#8211; What does O(log n) mean exactly? &#8211; Stack Overflow</a><span style="line-height: 1.5em;"> via stackoverflow.com</span>
</p>