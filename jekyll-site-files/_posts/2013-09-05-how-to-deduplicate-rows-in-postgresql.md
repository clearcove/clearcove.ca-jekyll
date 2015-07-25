---
title: How to deduplicate rows in PostgreSQL
author: jhund
layout: post
permalink: /2013/09/how-to-deduplicate-rows-in-postgresql/
categories:
  - Blog
---
Use the following query to keep only one row and delete all duplicates of that row.

Note: this is non-standard SQL and works only with PostgreSQL.

The following snippet finds all rows in the users table that have identical email and organization_id. It keeps the oldest of the rows (via users.id > u2.id) and deletes the newer duplicates.

<pre>DELETE FROM users USING users u2
 WHERE users.email = u2.email
 AND users.organization_id = u2.organization_id
 AND users.id &gt; u2.id;</pre>

Read the documentation for [PostgreSQL&#8217;s USING clause][1] in combination with DELETE.

&nbsp;

 [1]: http://www.postgresql.org/docs/9.2/static/sql-delete.html