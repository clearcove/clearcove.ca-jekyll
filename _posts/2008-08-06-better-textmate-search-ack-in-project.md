---
title: 'Better TextMate search: Ack in project'
author: jhund
layout: post
permalink: /2008/08/better-textmate-search-ack-in-project/
enclosure:
  - |
    http://protocool.com/demos/ack_tmbundle.mov
    5488018
    video/quicktime
categories:
  - Software
  - Tools
tags:
  - broadcast
---
Yesterday [Trevor][1] mentioned the [Ack TextMate bundle][2] he developed to provide better &#8220;Search in project&#8221;. I installed it and was instantly sold:

  * search is much faster using [ack][3]
  * the results list pops up instantly with the first hits, and then it just keeps growing as you start reviewing the results
  * it has an option to show context around the results (see screenshot below)
  * you can customize the search behavior on a per user or per project basis using a [.ackrc file][4]
  * it has an option to follow symlinks. Useful for me because I symlink rails into vendor for my apps. Normally I don&#8217;t want to include the rails source in my project search
  * search is limited to currently selected file/folder and its children. Useful for scoping searches.

<!--more-->

<div id="attachment_204" style="width: 310px" class="wp-caption alignnone">
  <a href="https://clearcove.ca/images/2008/08/ack-in-project1.jpg"><img src="https://clearcove.ca/images/2008/08/ack-in-project1-300x156.jpg" alt="The Ack results list showing context" title="ack-in-project" width="300" height="156" class="size-medium wp-image-204" /></a>

  <p class="wp-caption-text">
    The Ack results list showing context
  </p>
</div>

Trevor also provides a [screencast][5] that highlights features and usage.

**Installation**

<pre>cd ~/Library/Application\ Support/TextMate/Bundles
git clone git://github.com/protocool/ack-tmbundle.git Ack.tmbundle</pre>

**Custom .ackrc file**

By default ack does not search all file types related to Rails projects. So I added some extensions using the ~/.ackrc file:

<pre>--type-add=ruby=.haml,.rake,.rsel
</pre>

You have to tell ack to consider the .ackrc file: In the search dialog, open advanced options and check the corresponding check box.

Thanks Trevor!

**Update 2008-08-24**: Updated the screenshot with the &#8216;default&#8217; Textmate theme which looks a lot nicer.
**Update 2008-11-08**: Added example .ackrc file

 [1]: http://protocool.com
 [2]: http://github.com/protocool/ack-tmbundle/tree/master
 [3]: http://petdance.com/ack/
 [4]: http://github.com/protocool/ack-tmbundle/wikis/recognizing-files
 [5]: http://protocool.com/demos/ack_tmbundle.mov
