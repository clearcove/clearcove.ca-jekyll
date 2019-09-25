---
title: Dependency resolution
layout: post
permalink: /2019/09/dependency-resolution
---

A good tool for dependency resolution is Ruby's [TSort](https://rubydoc.info/stdlib/tsort/TSort) standard library module.

It used to be part of Bundler to resolve dependencies. Bundler now uses the [Molinillo Gem](https://github.com/CocoaPods/Molinillo) instead. However the approach hasn't really changed, Molinillo still uses TSort under the hood.

TSort implements topological sorting using Tarjan's algorithm for strongly connected components.

Here are some articles that illustrate how and where it can be used:

* [Ruby's TSort explained](https://www.klausrubrecht.com/rubys-tsort-explained/)
* [Dependency Sorting in Ruby with TSort](https://www.viget.com/articles/dependency-sorting-in-ruby-with-tsort/)
* [Getting to Know the Ruby Standard Library – TSort](https://endofline.wordpress.com/2010/12/22/ruby-standard-library-tsort/)
