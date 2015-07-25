---
title: Designing a Secure REST (Web) API with HMAC
author: jhund
layout: post
permalink: /2012/02/designing-a-secure-rest-web-api-with-hmac/
categories:
  - Blog
  - Curated News
---
Description of how to use HMAC for secure API access. Gist:

  * send the message payload, along with a cryptographic hash of the payload
  * client and server both know a private key which is part of the hash
  * server can compute hash itself to confirm authenticity of payload.

Link: [Designing a Secure REST (Web) API without OAuth][1]

 [1]: http://bit.ly/xg1itB