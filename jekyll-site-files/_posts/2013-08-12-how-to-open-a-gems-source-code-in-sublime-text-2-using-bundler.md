---
title: 'How to open a gem&#8217;s source code in Sublime Text 2 using Bundler'
author: jhund
layout: post
permalink: /2013/08/how-to-open-a-gems-source-code-in-sublime-text-2-using-bundler/
categories:
  - Blog
  - Note to self
  - Ruby/Rails
  - Tools
---
Update: Even easier than the method described below is to set your EDITOR environment variable to subl and use

<pre>bundle open devise</pre>

This command opens the &#8216;devise&#8217; gem in Sublime Text 2. I use Bundler to manage gems:

<pre>bundle show devise | xargs sub</pre>

And as an aside, while researching the bundle open command, I also ran into

<pre>bundle console</pre>

which opens a Ruby IRB session with all the gems in your bundle loaded. Great if you need a console where you want your gems, but you don&#8217;t want to load the Rails app.