---
title: Tablet pressure support on OS X for X11, Gimp, and Inkscape
author: jhund
layout: post
permalink: /2008/12/tablet-pressure-support-on-os-x-for-x11-gimp-and-inkscape/
categories:
  - Note to self
  - Tools
---
Here is what I had to do to enable tablet pressure support for the X11 based Gimp and Inkscape applications on OS X Leopard:

My setup:

  * OS X Leopard: 10.5.5 on Intel
  * X11: [XQuartz 2.3.2 RC2][1]
  * [Inkscape][2]: 0.46 (Mac binary)
  * [Gimp][3]: 2.6.3 (Mac binary)
  * Tablet: Wacom Graphire ET from around 2003

Key points are:

  * get at least the 2.3.2 version of XQuartz. 2.3.1 will not work!
  * In Gimp/Edit/Preferences/Input Devices: Click on &#8220;Configure extended input devices&#8230;&#8221;, set device &#8216;pen&#8217; to &#8216;screen&#8217;.
  * In Inkscape/File/Input Devices &#8230;: Set device &#8216;pen&#8217; to &#8216;screen&#8217;.
  * You might have to restart your apps

Nice. Now we have pressure support on OS X for these two great apps. The Inkscape calligraphy tool is very nice. Makes my handwriting look nicer than on paper.

 [1]: http://static.macosforge.org/xquartz/downloads/
 [2]: http://www.inkscape.org
 [3]: http://www.gimp.org/