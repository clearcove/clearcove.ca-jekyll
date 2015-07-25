---
title: 'Note to self: How to show hidden folders in OS X file dialogs'
author: jhund
layout: post
permalink: /2013/01/note-to-self-how-to-show-hidden-folders-in-os-x-file-dialogs/
categories:
  - Note to self
  - Tools
---
I was trying to specify a custom SSH key to connect to a remote database via SSH tunnel with my database client. The standard OS X file dialog doesn&#8217;t show hidden folders, so I couldn&#8217;t select the custom key file in the ~/.ssh folder.

Turns out it&#8217;s quite simple. Press the following keys to toggle visibility of hidden folders in OS X file dialogs:

<pre>&lt;command&gt; + &lt;shift&gt; + '.'</pre>

That last character is a period.