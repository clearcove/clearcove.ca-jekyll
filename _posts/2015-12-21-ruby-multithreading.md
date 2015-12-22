---
title: Ruby multithreading
layout: post
permalink: /2015/12/ruby-multithreading
---

The blog post [How do I know whether my Rails app is thread safe or not](https://bearmetal.eu/theden/how-do-i-know-whether-my-rails-app-is-thread-safe-or-not/) gives a good list of criteria to check if you want to write thread safe code:

* Global variables: Don’t use them.
* Class variables (`@@foo`): You shouldn’t mutate them, so why not use constants instead?
* Class instance variables (`@foo`): Both class variables and class instance variables can be set by class methods…
* Memoization: Not an issue by itself. Beware not to use it on class methods, and also remember that `||=` is not an atomic operation, so you may get race conditions.
* Constants: Don’t reassign them, and remember that `CONST = [1,2,3]` freezes the reference to the array, however not the contents of the array itself. So to be safe, freeze the array: `CONST = [1,2,3].freeze`. Remember that freeze is shallow!
* Environment variables: Same as constants.
* 

> The main thing to keep in mind is to never mutate data that is shared across threads. Most often this happens through class variables, class instance variables, or by accidentally mutating objects that are referenced by a constant.
