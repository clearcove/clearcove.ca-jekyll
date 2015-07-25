---
title: Simplest way to use cron with Rails
author: jhund
layout: post
permalink: /2009/03/simplest-way-to-use-cron-with-rails/
syntaxhighlighter_encoded:
  - 1
categories:
  - Note to self
  - Ruby/Rails
tags:
  - broadcast
---
For simple applications I use cron to run automated background jobs like sending emails or indexing sphinx.

I like to have all aspects of my application under version control. Cron is no exception. To do this, I add a file named &#8220;crontab&#8221; in /config. In there I add all my cron jobs in regular cron notation.

<!--more-->

  
Then I add something like this to my capistrano deploy recipe:

<pre class="brush: ruby; title: ; notranslate" title="">task :after_update_code, :roles =&gt; :app do
  # Install crontab
  # IMPORTANT: works only if this is the only instance of app
  # running under this user account
  run "crontab -u #{user} #{release_path}/config/crontab"
end
</pre>

Now every time I deploy, capistrano updates the deploy user&#8217;s crontab with the most current version.

## When it doesn&#8217;t work as expected&#8230;

&#8230; here are the places to look:

First **run the cron command on the command line**. Run it as the same user as cron would run it. I typically use my deploy user to do everything: web server user, capistrano user, ssh user, cron user. This helps avoid issues with permissions. I also keep the app in the deploy user&#8217;s home directory.

If the command executes fine on the command line, however it doesn&#8217;t from cron, then there is a good chance that you have **differences in PATH or SHELL between cron and command line**. To address this, set both in the crontab:

<pre class="brush: bash; title: ; notranslate" title="">PATH=(your path...)
SHELL=(your shell...)
</pre>

To find out your deploy user&#8217;s PATH and SHELL run these two commands as deploy user on the production server&#8217;s command line:

<pre class="brush: bash; title: ; notranslate" title="">echo $PATH
echo $SHELL
</pre>