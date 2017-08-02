---
title: Ruby puts debugging
layout: post
permalink: /2017/08/ruby-puts-debugging
---

Aaron Patterson calls himself a "[puts debuggerer](https://tenderlovemaking.com/2016/02/05/i-am-a-puts-debuggerer.html)". His article lists useful techniques for debugging ruby code using `puts`:

* I know where I am but not how I got here: `puts caller` will print the call stack.
* I’m calling a method, but I don’t know where it goes: `p method(:render).source_location` will show you where the method `render` is defined.
* I’m calling super but I don’t know where that goes: `p method(:render).super_method.source_location` will show you where method `render`'s super is defined.
* An object is being mutated, but I don’t know where: `#freeze` the object to trigger an exception (with backtrace) when it gets mutated.
* I have a deadlock, but I don’t know where: Inside the thread call `p t; puts t.backtrace` to get insight.

And then there is always the `byebug` gem with `next`, `step`, etc. when puts is not powerful enough.
