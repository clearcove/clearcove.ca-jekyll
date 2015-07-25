---
title: 'Use &#8220;DATA&#8221; to combine data and code in a single ruby file'
author: jhund
layout: post
permalink: /2013/01/use-data-to-combine-data-and-code-in-a-single-ruby-file/
categories:
  - Blog
---
> <p style="margin: 0px 0px 1.5em; padding: 0px; border: 0px; font-size: 12px; font-family: 'lucida grande', calibri, 'liberation sans', verdana, arial, helvetica, sans-serif; vertical-align: baseline; line-height: 18px;">
>   <code style="margin: 1.5em 0px; padding: 0px; border: 0px; font-family: 'bitstream vera sans mono', monaco, 'lucida console', 'courier new', courier, serif; vertical-align: baseline; white-space: pre; line-height: normal;">DATA</code>is in fact an IO object, that you can read from (or anything else you&#8217;d do with an IO object). It contains all the content after<code style="margin: 1.5em 0px; padding: 0px; border: 0px; font-family: 'bitstream vera sans mono', monaco, 'lucida console', 'courier new', courier, serif; vertical-align: baseline; white-space: pre; line-height: normal;">__END__</code>in that ruby script file<a style="margin: 0px; padding: 0px; border: 0px; font-style: inherit; font-family: inherit; vertical-align: baseline; outline: none; color: #555555;" href="http://caiustheory.com/why-i-love-data#fn1">*</a>. (It only exists when the file contains<code style="margin: 1.5em 0px; padding: 0px; border: 0px; font-family: 'bitstream vera sans mono', monaco, 'lucida console', 'courier new', courier, serif; vertical-align: baseline; white-space: pre; line-height: normal;">__END__</code>, and for the first file ruby invokes though. See<a style="margin: 0px; padding: 0px; border: 0px; font-style: inherit; font-family: inherit; vertical-align: baseline; outline: none; color: #555555;" href="http://caiustheory.com/why-i-love-data#fn1">footnote</a>for more details.)
> </p>
> 
> <p style="margin: 0px 0px 1.5em; padding: 0px; border: 0px; font-size: 12px; font-family: 'lucida grande', calibri, 'liberation sans', verdana, arial, helvetica, sans-serif; vertical-align: baseline; line-height: 18px;">
>   How can we use this, and why indeed do I love this fickle constant? I mostly use it for quick scripts where I need to process text data, rather than piping to STDIN.
> </p>

<div>
  An example:
</div>

<pre style="margin: 1.5em 0px; padding: 1em; border: 1px solid #eeeeee; font-size: 12px; font-family: 'bitstream vera sans mono', monaco, 'lucida console', 'courier new', courier, serif; vertical-align: baseline; line-height: normal; overflow: auto;"><code style="margin: 1.5em 0px; padding: 0px; border: 0px; font-family: 'bitstream vera sans mono', monaco, 'lucida console', 'courier new', courier, serif; vertical-align: baseline;">DATA.each_line.map(&:chomp).each do |url| `open "#{url}"` end __END__ http://google.com/ http://yahoo.com/</code></pre>

Link: [Why I love DATA &#8211; Caius Theory][1] via caiustheory.com

 [1]: http://bit.ly/V0eQHl