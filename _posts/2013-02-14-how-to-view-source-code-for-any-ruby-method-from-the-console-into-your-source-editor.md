---
title: How to view source code for any ruby method from the console into your source editor
author: jhund
layout: post
permalink: /2013/02/how-to-view-source-code-for-any-ruby-method-from-the-console-into-your-source-editor/
categories:
  - Blog
---
Mike Clark from Pragmatic Studio has a great tip for viewing any ruby method&#8217;s source code straight from the console.

Just define this method:

<pre>def source_for(object, method)
  location = object.method(method).source_location
  `subl #{location[0]}:#{location[1]}` if location
  location
end</pre>

and then use it like so:

<pre>&gt;&gt; source_for(Person.new, :update)</pre>

Link: [The Pragmatic Studio | &#8220;View Source&#8221; On Ruby Methods][1] via pragmaticstudio.com

 [1]: http://pragmaticstudio.com/blog/2013/2/13/view-source-ruby-methods