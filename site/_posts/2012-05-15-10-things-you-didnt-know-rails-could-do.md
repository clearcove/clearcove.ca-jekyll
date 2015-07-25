---
title: '10 Things You Didn&#8217;t Know Rails Could do'
author: jhund
layout: post
permalink: /2012/05/10-things-you-didnt-know-rails-could-do/
categories:
  - Blog
  - Curated News
---
James Edward Gray 2 gave a presentation about hidden skills of Rails. Here are the ones I found most interesting:

  1. Run from a single file
  2. Remind you of things via &#8220;rake notes&#8221;
  3. Sandbox the console via &#8220;rails console &#8211;sandbox&#8221; (reverts all changes on exit)
  4. Run helper methods in the console via &#8220;helper.number\_to\_currency(100)&#8221;
  5. Use non-webrick servers in development via &#8220;rails s thin&#8221; (this is shorter than &#8220;bundle exec thin start&#8221;!)
  6. Show you the DB&#8217;s migration status via &#8220;rake db:migrate:status&#8221; to see which migrations have run.
  7. Pluck fields from the DB: &#8220;User.pluck(:email)&#8221; instead of &#8220;User.select(:email).map(&:email)&#8221;
  8. Count DB records in groups via &#8220;Event.group(:trigger).count&#8221; => { &#8220;edit&#8221; => 3, &#8220;view&#8221; => 10 }
  9. Override associations via &#8220;belongs\_to :owner; def owner=(new\_owner); self.previous_owner = owner; super; end&#8221;
 10. Use limitless strings in PostgreSQL (via Josh Susser)
 11. Use a different database for each user.
 12. Merge nested hashes via Hash#deep_merge
 13. Remove specific keys from a hash via &#8220;params.except(:controller, :action)&#8221;

Link: [10 Things You Didn&#8217;t Know Rails Could do // Speaker Deck][1] via speakerdeck.com

 [1]: http://bit.ly/ICV2iO