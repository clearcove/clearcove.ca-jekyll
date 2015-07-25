---
title: Goodbye WordPress, hello Jekyll
layout: post
permalink: /2015/07/goodbye-wordpress-hello-jekyll
---
Finally I got around to moving the clearcove.ca website from WordPress to [Jekyll](http://jekyllrb.com/).

I look forward to simple blogging in [Markdown format](https://en.wikipedia.org/wiki/Markdown) and no more site outages because of WordPress hosting issues. It's plain text files all the way down now.

The migration was pretty painless:

* Exported WordPress site using the [Jekyll exporter WP plugin](https://github.com/benbalter/wordpress-to-jekyll-exporter).
* Renamed `wp-content/uploads/` &rarr; `images/`.
* Moved all pages from `about-clearcove.md` &rarr; `about-clearcove/index.md` for clean URLs.
* Ported the layout markup from WP to Jekyll.
* Pushed it all to Github pages.
* Updated my `A` name DNS record.
* Done, and no URLs broken.
