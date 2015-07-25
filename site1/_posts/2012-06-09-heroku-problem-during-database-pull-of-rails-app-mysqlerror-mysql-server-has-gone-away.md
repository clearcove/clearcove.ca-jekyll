---
title: 'Heroku Problem During Database Pull of Rails App: Mysql::Error MySQL server has gone away'
author: jhund
layout: post
permalink: /2012/06/heroku-problem-during-database-pull-of-rails-app-mysqlerror-mysql-server-has-gone-away/
categories:
  - Blog
  - Curated News
---
This fixed an error I got when pulling a rails database from Heroku via taps:

command: heroku db:pull

error:&nbsp;MySQL server has gone away (Mysql::Error)

Fix: add the following to my.cnf file (location depends on OS):

<pre>[mysql]</pre>

<pre>max_allowed_packet=32M</pre>

<pre>[mysqld]</pre>

<pre>max_allowed_packet=32M</pre>

Link: [Heroku Problem During Database Pull of Rails App: Mysql::Error MySQL server has gone away &#8211; Stack Overflow][1] via stackoverflow.com

 [1]: http://bit.ly/L9xWlD