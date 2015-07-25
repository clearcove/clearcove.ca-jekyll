---
title: 'github &#8211; Git push requires username and password &#8211; Stack Overflow'
author: jhund
layout: post
permalink: /2012/06/github-git-push-requires-username-and-password-stack-overflow/
categories:
  - Blog
  - Curated News
---
Here is what to do when you suddenly have to provide username and password when pushing to github:

> git remote set-url origin&nbsp;git@github.com:[username]/[reponame].git

The reason is that you likely used the https URL for the github remote on your repo. This happened to me after github changed their UI and made the https URL the default. There is a button to switch it to SSH.

Link: [github &#8211; Git push requires username and password &#8211; Stack Overflow][1] via stackoverflow.com

 [1]: http://bit.ly/KG7Zr7