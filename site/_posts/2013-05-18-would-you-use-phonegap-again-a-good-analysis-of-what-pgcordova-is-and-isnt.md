---
title: 'Would you use PhoneGap again? A good analysis of what PG/Cordova is and isn&#8217;t'
author: jhund
layout: post
permalink: /2013/05/would-you-use-phonegap-again-a-good-analysis-of-what-pgcordova-is-and-isnt/
categories:
  - Blog
---
A developer who built an app with PhoneGap/Cordova summarizes his experience and explains what you can reasonably expect when building mobile apps with PG/Cordova:

<blockquote class="iii-article-quote">
  <p>
    The biggest misunderstanding with Apache Cordova is that it does *everything* for you. But it’s not the case. It is just the base for your application. It supports you to package your app and helps you to access device features like the camera. Nothing more. There is nothing in Cordova which helps you to organize your App in terms of for example MVC. It’s not an application framework.
  </p>
</blockquote>

There is currently a big opportunity for filling the gap: A framework on top of Cordova that:

  * is very lightweight so that it performs well. Can be accomplished by having only very limited browser support
  * comes with a UI library that emulates the look and feel of its host operating system, primarily iOS and Android
  * provides an application framework, e.g., MVC or comparable.

<div>
  This could be accomplished by stripping down jQuery UI and combine it with a subset of jQuery, e.g., zepto.js.
</div>

<div>
</div>

<p class="iii-article-source">
  Link: <a href="http://www.grobmeier.de/would-you-use-phonegap-again-18052013.html">Would you use PhoneGap again? | Christian Grobmeier Solutions</a> via www.grobmeier.de
</p>