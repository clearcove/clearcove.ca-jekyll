---
title: How to terminate a stuck Postgres query
author: jhund
layout: post
permalink: /2013/06/how-to-terminate-a-stuck-postgres-query/
categories:
  - Blog
---
Sometimes a long running query in Postgres can get stuck and you need to kill it. This recipe outlines the steps to do so. A few notes:

  * <span style="line-height: 13px;">This recipe does not require console access to the server. </span>
  * <span style="line-height: 13px;">You need a way to run a postgresql command. I used Navicat. psql or a Rails console work, too.</span>
  * <span style="line-height: 13px;">Tested on Postgresql 9.2 on heroku postgres.<br /> </span>

And here are the instructions:

  1. Get a list of all currently active connections on the Postgres server. Run the query below on the server:  
    `SELECT * FROM pg_stat_activity ORDER BY client_addr ASC, query_start ASC`
  2. Each row in the results table corresponds to a postgres process. Find the row for the process you want to kill by looking at the &#8216;current_query&#8217; column.
  3. Record the process id. You can find it in the &#8216;procpid&#8217; column from the row that contains the query you want to kill.
  4. Kill the process by running the following query (replace &#8216;<pid>&#8217; with the process id you found in step 3):  
    `SELECT pg_cancel_backend(<pid>)`
  5. Reload the query from step 1 to confirm that the process is gone.