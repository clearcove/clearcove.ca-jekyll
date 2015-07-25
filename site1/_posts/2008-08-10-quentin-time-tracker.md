---
title: 'OpenSource project: Quentin Time Tracker'
author: jhund
layout: post
permalink: /2008/08/quentin-time-tracker/
syntaxhighlighter_encoded:
  - 1
categories:
  - Ruby/Rails
  - Tools
---
Being a contract software developer, tracking my time is extremely important for my business: I need to write invoices with task break downs, I want to get better at estimating, and I am just curious about what I spend my time on.

So I filled this need by writing my own Rails time tracker: meet Quentin. I cleaned it up a bit and just published it as an MIT-licensed [open source project on github][1].

<div id="attachment_265" style="width: 310px" class="wp-caption alignnone">
  <img src="http://clearcove.ca/wp-content/uploads/2008/10/quentin2-300x161.jpg" alt="Quentin dashboard" title="Quentin time tracker" width="300" height="161" class="size-medium wp-image-265" />
  
  <p class="wp-caption-text">
    Quentin dashboard
  </p>
</div>

<!--more-->

<p class="clear">
  There are a lot of time trackers available. Why did I write my own?
</p>

  * This is mission critical data. I want full control and access. Time is my most valuable resource, so I need to understand it.
  * Track the roles I take on: SW architect, sysadmin, manager, SW developer, entrepreneur, expert, &#8230; Quentin gives me a detailed breakdown for each role, broken down by projects and time frames. This is useful to maximize my resource utilization and to plan and measure my professional development.
  * Knowing when it is enough. When you freelance, it is easy to become restless. Quentin tells me when I have made enough money to meet my revenue targets (daily, weekly and yearly). Since I am a visual person, I had to add charts that tell me where I am at (See screenshot below)
  * Create invoices: Quentin generates invoices with detailed work break downs for my clients. No extra work required.

<div id="attachment_267" style="width: 310px" class="wp-caption alignnone">
  <img src="http://clearcove.ca/wp-content/uploads/2008/10/quentin-300x132.jpg" alt="Quentin hourly charts" title="Quentin hourly charts" width="300" height="132" class="size-medium wp-image-267" />
  
  <p class="wp-caption-text">
    Quentin hourly charts
  </p>
</div>

<p class="clear">
  It&#8217;s easy to use: create a new task, hit enter, and the clock is ticking. Interruption? No problem. Hit stop, or just create a new task. Forget to stop the timer? Just edit the task. I have added detailed auditing so that I can measure my working hours accurately.
</p>

The screen shot below shows the Quentin dashboard:

<div id="attachment_146" style="width: 310px" class="wp-caption alignnone">
  <a href="http://clearcove.ca/wp-content/uploads/2008/08/quentin-dashboard1.jpg"><img src="http://clearcove.ca/wp-content/uploads/2008/08/quentin-dashboard1-300x124.jpg" alt="Quentin dashboard" title="quentin-dashboard" width="300" height="124" class="size-medium wp-image-146" /></a>
  
  <p class="wp-caption-text">
    Quentin dashboard
  </p>
</div>

## Reporting

When you track your activities every day, over the course of a year, you have a very rich pool of data to work with. With reporting, I can know how much time I spend &#8230;

  * on a given project &#8211; so that I can invoice my clients
  * fulfilling a certain role &#8211; so that I can plan and measure my professional development
  * per week working &#8211; so that I can work towards a sustainable work load

The other things I can learn from the data:

  * How am I doing for revenue this year?
  * Which projects are falling behind and need attention?

 [1]: http://github.com/jhund/quentin