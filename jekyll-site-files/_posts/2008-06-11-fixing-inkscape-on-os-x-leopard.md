---
title: Fixing Inkscape on OS X Leopard
author: jhund
layout: post
permalink: /2008/06/fixing-inkscape-on-os-x-leopard/
categories:
  - Note to self
---
After I upgraded to Leopard, Inkscape did not start up any more. Just a blank screen. Here is what I did to fix it:

> If you like <a title="Inkscape" onclick="javascript:urchinTracker('/outbound/www.inkscape.org/');" href="http://www.inkscape.org/" target="_blank">Inkscape</a> (an open source alternative to Adobe Illustrator) and have a Mac with Leopard; you will figure that it after installation it will not work. It will say that font caching might take time, but never start. It’s because of a simple bug, with a simple solution. Just open a terminal (Applications/Utilities) and type this, followed by an enter;
> 
> `mkdir ~/.fontconfig`  
> After that it should start normally.

Thanks to [Kuneri][1] for this tip.

 [1]: http://bloggy.kuneri.net/2008/05/14/how-to-run-inkscape-on-mac-leopard/