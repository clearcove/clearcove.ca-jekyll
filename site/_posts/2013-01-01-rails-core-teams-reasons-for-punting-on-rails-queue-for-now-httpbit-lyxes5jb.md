---
title: 'Rails core team&#8217;s reasons for punting on Rails.queue for now: http://bit.ly/XeS5jB'
author: jhund
layout: post
permalink: /2013/01/rails-core-teams-reasons-for-punting-on-rails-queue-for-now-httpbit-lyxes5jb/
categories:
  - Blog
---
Rails core team decided to not include Rails.queue in the 4.0 release. @jeremy has a nice writeup for the reasons:

  * Resque&#8217;s interface is currently easier to use than Rails.queue&#8217;s
  * Marshalling has some issues
  * Rails.queue is abstracting at the wrong level (equivalent to imaginary Rails.database)
  * Not efficient when working with Russian doll caching
  * There might be a better way to avoid marshalling and Job classes alltogether by processing certain jobs after the response has been sent to client. However this requires work on the Rack integration.

Link: [Move background jobs to the &#8216;jobs&#8217; branch until fully baked. Not shippin&#8230; &middot; f9da785 &middot; rails/rails][1] via github.com

 [1]: http://bit.ly/XeS5jB