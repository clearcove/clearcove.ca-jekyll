---
title: How to make Ruby class methods backgroundable in torquebox
author: jhund
layout: post
permalink: /2012/09/how-to-make-ruby-class-methods-backgroundable-in-torquebox/
categories:
  - Blog
  - Curated News
---
The link below explains how to make a Ruby instance method backgroundable in Torquebox. Tobey Crawley told me via the mailing list how to make class methods backgroundable by including the Backgroundable Module into the class&#8217; singleton class:

<pre>class Thing</pre>

&nbsp; class << self

&nbsp; &nbsp; include TorqueBox::Messaging::Backgroundable

&nbsp; &nbsp; def foo

&nbsp; &nbsp; &nbsp; # do something

&nbsp; &nbsp; end

&nbsp; &nbsp; always_background :foo

&nbsp; end

end

Thing.foo # will always be backgrounded.

Note: make sure to call always_background after the method is defined. There is a ticket to fix this limitation: https://issues.jboss.org/browse/TORQUE-891.

If you want to use the background proxy method instead of always backgrounding foo (i.e. Thing.background.foo) you can just leave out the always_background call altogether.

Link: [Chapter&nbsp;8.&nbsp;TorqueBox Messaging][1] via torquebox.org

 [1]: http://bit.ly/TgsJyN