---
title: Model complicated logic with a Decision table + some machine learning goodness
author: jhund
layout: post
permalink: /2013/05/model-complicated-logic-with-a-decision-table-some-machine-learning-goodness/
categories:
  - Blog
---
<p class="iii-article-excerpt">
  Decision tables can be used to model complicated logic. Like flowcharts, nested if-statements, and case statements, they associate conditions with actions to perform, but often in a more elegant and concise way.
</p>

<p class="iii-article-excerpt">
  <span style="line-height: 1.5em;">A Decision table has four quadrants:</span>
</p>

<li class="iii-article-excerpt">
  <span style="line-height: 1.5em;">Conditions</span>
</li>
<li class="iii-article-excerpt">
  <span style="line-height: 1.5em;">Condition alternatives</span>
</li>
<li class="iii-article-excerpt">
  <span style="line-height: 1.5em;">Actions</span>
</li>
<li class="iii-article-excerpt">
  <span style="line-height: 1.5em;">Action entries</span>
</li>

<div>
  Like finite state machines, they provide an explicit and complete overview of the problem space with all possible permutations. Nested if statements can be rendered from Decision tables.
</div>

<div>
</div>

### Resources

<div>
  <ul>
    <li>
      <a href="https://github.com/jmettraux/rufus-decision">rufus-decision</a> &#8211; A Ruby gem to act on Decision tables provided in CSV format (e.g., from spreadsheet or google docs).
    </li>
    <li>
      Ilya Grigorik has a good <a href="http://www.igvita.com/2007/04/16/decision-tree-learning-in-ruby/">blog post about Decision trees</a> (not to be confused with Decision tables!). His <a href="https://github.com/igrigorik/decisiontree">decisiontree</a> gem allows one to analyse the rules and extract a visual representation.
    </li>
  </ul>
</div>

<p class="iii-article-excerpt">
  <span style="line-height: 1.5em;">Link: </span><a style="line-height: 1.5em;" href="http://en.wikipedia.org/wiki/Decision_tables">Decision table &#8211; Wikipedia, the free encyclopedia</a><span style="line-height: 1.5em;"> via en.wikipedia.org</span>
</p>