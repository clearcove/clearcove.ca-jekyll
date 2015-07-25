---
title: 10 Easy Steps to a Complete Understanding of SQL
author: jhund
layout: post
permalink: /2013/09/10-easy-steps-to-a-complete-understanding-of-sql/
categories:
  - Blog
---
The article has some good insights for truly understanding SQL as a declarative programming language.

#### The lexical order of a SQL statement is different from the order of execution.

The key insight is that SELECT comes fairly late in the order of execution. That means other parts can&#8217;t reference stuff that was declared in the SELECT clause.

  * Lexical order: SELECT [ DISTINCT ], FROM, WHERE, GROUP BY, HAVING, UNION, ORDER BY
  * Order of execution: FROM, WHERE, GROUP BY, HAVING, SELECT, DISTINCT, UNION, ORDER BY

#### SQL is primarily about table references

The first (FROM) part of executing a SQL statement is building (possibly complex) table references. The &#8220;output&#8221; of the FROM clause is a combined table reference of the combined degree of all table references.

#### Prefer explicit JOIN clauses over comma separated FROM items

When building table references using FROM, prefer to explicitly state all JOIN clauses rather than relying on comma separated table references. This will help avoid cartesian products of tables and makes the intent more clear since the join conditions are next to the JOIN statement and not further away in the WHERE clause.

#### Derived tables (e.g., sub queries) are like variables

You can think of sub queries as variables. If you declare them early on, then you can reference them in all clauses.

#### GROUP BY transforms previous table references

> If you apply GROUP BY, then you reduce the number of available columns in all subsequent logical clauses &#8211; including SELECT. This is the syntactical reason why you can only reference columns from the GROUP BY clause in the SELECT clause. Note that other columns may still be available as arguments of aggregate functions.

#### SELECT is a projection

> Once you&#8217;ve generated your table reference, filtered it, transformed it, you can step to projecting it to another form. The SELECT clause is like a projector. A table function making use of a row value expression to transform each record from the previously constructed table reference into the final outcome.

Link: [10 Easy Steps to a Complete Understanding of SQL &#8211; Tech.Pro][1] via tech.pro

 [1]: http://tech.pro/tutorial/1555/10-easy-steps-to-a-complete-understanding-of-sql