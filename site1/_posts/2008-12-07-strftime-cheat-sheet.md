---
title: strftime cheat sheet
author: jhund
layout: post
permalink: /2008/12/strftime-cheat-sheet/
other_media:
  - 
main_video:
  - 
categories:
  - Software
  - Tools
---
I have used strftime a lot lately on one of my projects. I could not find a user friendly reference for strftime, so I wrote one. [Here is my user friendly strftime cheat sheet in PDF format][1]:

<div class="mceTemp">
  <dl id="attachment_347" class="wp-caption alignnone" style="width: 310px;">
    <dt class="wp-caption-dt">
      <a href="http://clearcove.ca/wp-content/uploads/2008/12/strftime_reference.pdf"><img class="size-medium wp-image-347" title="strftime reference" src="http://clearcove.ca/wp-content/uploads/2008/12/strftime_ref-300x104.jpg" alt="strftime reference" width="300" height="104" /></a>
    </dt>
  </dl>
</div>

<!--more-->

In case you are wondering what strftime does: It allows you to represent a date/time as a formatted string. Example:

<pre class="brush: ruby; title: ; notranslate" title="">Time.now.strftime('%A, %l:%M %p')
=&gt; "Friday,  9:06 AM"
</pre>

And here is some handy Ruby code to look at the options for strftime:

<pre class="brush: ruby; title: ; notranslate" title="">def print_line(char)
  puts "%#{char}: #{Time.now.strftime("%#{char}")}"
end

('a'..'z').each do |char|
  print_line(char)
  print_line(char.upcase)
end
</pre>

 [1]: http://clearcove.ca/wp-content/uploads/2008/12/strftime_reference.pdf