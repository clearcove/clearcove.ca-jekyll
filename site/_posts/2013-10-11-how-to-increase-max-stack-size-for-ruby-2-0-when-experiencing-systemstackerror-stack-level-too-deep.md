---
title: 'How to increase max stack size for Ruby 2.0 when experiencing (SystemStackError) &#8220;stack level too deep&#8221;'
author: jhund
layout: post
permalink: /2013/10/how-to-increase-max-stack-size-for-ruby-2-0-when-experiencing-systemstackerror-stack-level-too-deep/
categories:
  - Blog
---
We process a lot of HTML documents using Nokogiri. On large HTML files we sometimes get the (SystemStackError) &#8220;stack level too deep&#8221; exception. This happens because Nokogiri recursively processes the dom, and on large documents it exceeds the maximum stack size of 1024 KB on our system.

In Ruby versions before Dec. 2012, the Ruby stack size was limited by the C stack size which can be changed using the &#8216;ulimit -s&#8217; command. In Ruby 2.0 the stack size is now limited via its own environment variable RUBY\_THREAD\_VM\_STACK\_SIZE.

We address the problem by setting this env variable to a higher value when launching our ruby processes.

Here is some code to try this yourself:

First launch a Ruby console

<pre>irb</pre>

Then inspect your current max stack size

<pre>irb(main):052:0&gt; ENV['RUBY_THREAD_VM_STACK_SIZE']
=&gt; "1048576"</pre>

You can also look at all the default settings for the VM:

<pre>irb(main):001:0&gt; RubyVM::DEFAULT_PARAMS
=&gt; {:thread_vm_stack_size=&gt;1048576,
    :thread_machine_stack_size=&gt;1048576,
    :fiber_vm_stack_size=&gt;131072,
    :fiber_machine_stack_size=&gt;524288}</pre>

Now run the program below and modify the upper limit from 7600 to 7700. You will see it work with 7600 and exit with SystemStackError for 7700:

<pre>upper_limit = 7600 # 7600 works, 7700 doesn't

def so(arg, count)
  r = if count &lt; upper_limit
    ('A') + so(arg, count + 1)
  else
    arg
  end
  r
end

so('a', 0)</pre>

In order to make it work with 7700, quit your irb and re-launch it with modified max stack size:

<pre>export RUBY_THREAD_VM_STACK_SIZE=2000000
irb</pre>

Link: [[ruby-cvs:45648] ko1:r38478 (trunk): * vm.c: support variable VM/Machi][1] via permalink.gmane.org

 [1]: http://permalink.gmane.org/gmane.comp.lang.ruby.cvs/41626