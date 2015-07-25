---
title: Cross browser testing Internet Explorer on Mac OS X
author: jhund
layout: post
permalink: /2013/04/cross-browser-testing-internet-explorer-on-mac-os-x/
categories:
  - Note to self
  - Tools
---
My development environment is OS X. In order to do cross-browser testing with Internet Explorer (IE) on MS Window, I use the following tools:

  * [<span style="line-height: 13px;">Oracle VirtualBox</span>][1]
  * [ievms][2]

#### Start ievms virtual boxes right away!

<span style="line-height: 13px;">When installing ievms, I had to launch the images right away. In one case I just installed them and let them sit for more than 30 days. In this scenario I could not restore them to the 30 day trial window.</span>

#### Set up network

When testing, I want to connect to the local web server running on my OS X host system. In order to do so, there needs to be network connectivity between the Windows guest and the OS X host. I found it worked best when I choose &#8216;Bridged Adapter&#8217; and then access the web server on the OS X host via its local network IP address.

Choose the networking mode:

  1. Select the Virtual box image
  2. Select &#8216;Settings&#8217;
  3. Select &#8216;Network&#8217; tab
  4. Check &#8216;Enable Network Adapter&#8217;
  5. Select Attached to: &#8216;Bridged Adapter&#8217;
  6. Select the right device (the one that connects to your router so that it can assign an IP address to your Windows guest machine)
  7. Click &#8216;Ok&#8217;
  8. Now start your windows guest machine
  9. You can review IP addresses: Windows: Start / Command prompt / &#8220;ipconfig /all&#8221;; OS X: in console type &#8220;ifconfig -a&#8221;; Both systems should share the same subnet in order to connect.
 10. Ping your host: &#8220;ping 192.168.1.111&#8243;
 11. To connect to your local web server on the OS X host: Start Internet Explorer, type the following into your address bar: &#8220;http://192.168.1.111:3000&#8243; (adjust the IP address and server port as needed).  
    **IMPORTANT**: I found that it did not work unless I prefixed the address with &#8216;http://&#8217;. I also had to wait quite a bit after boot, and I also pinged the host. Eventually it always worked. Not sure which of these three made it happen&#8230;

Here we go, 2 more Yaks shaved.

 [1]: https://www.virtualbox.org/
 [2]: https://github.com/xdissent/ievms