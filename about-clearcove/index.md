---
title: About ClearCove Software Inc.
layout: page
---
<p class="lead unconstrained" style="text-align: left;">
  We design, develop and maintain full-stack online software systems. We cover the entire range from highly usable frontend apps to powerful back-end server software and everything in between. We are a global company, working with clients in North America and Europe.
</p>

<div class="row-fluid">
  <div class="span8 offset2">
    <div class="well">
      <div class="marginalia_right">
        <img class="img-polaroid" style="width: 200px;" alt="Jo Hund" src="/images/2008/07/portraitjo_medium-300x225.jpg" />
        <div class="img_caption">
          Hi there, I&#8217;m Jo Hund
        </div>
      </div>

      <p class="unconstrained">
        I started professional software development in 1998 and founded ClearCove Software Inc. in 2006. Outside of work I enjoy outdoor pursuits (hiking, trail running and open water rowing) and gardening. I live with my family in the beautiful Comox Valley on Vancouver Island, Canada. 
      </p>

      <p>
        Jo Hund, founder of ClearCove Software, Inc.
      </p>
    </div>
  </div>
</div>

<div class="row-fluid">
  <div class="span6">
    <h3>
      We offer the following services:
    </h3>
    <ul>
      <li>
        Software architecture and development
      </li>
      <li>
        Hosting and deployment management
      </li>
      <li>
        Software maintenance
      </li>
      <li>
        Technical support
      </li>
      <li>
        Performance optimization
      </li>
      <li>
        Project management
      </li>
      <li>
        Code reviews
      </li>
    </ul>

    <h3>
      Clients
    </h3>
    <p>
      <span style="line-height: 1.5em;">We are privileged to work with fantastic commercial, non-profit, and government clients in North America and Europe. Below is a selection of projects we have developed and continue to support:</span>
    </p>
    <ul>
      <li>
        Marketing automation - web scraping, information extraction, advanced search, machine learning.
      </li>
      <li>
        Condo board/Strata council management - communications, document management, scheduling, issue tracking.
      </li>
      <li>
        Publishing automation – document management, conversion, and validation.
      </li>
      <li>
        Online training – courseware authoring, registration and training facilitation.
      </li>
      <li>
        Conference management – registration, reporting, and workflow automation.
      </li>
      <li>
        Online Questionbank – allows publishing staff to author questions, and teachers to find, edit and compile questions into student practice tests.
      </li>
      <li>
        Government financial reporting – legacy data import, ongoing data import, report generation.
      </li>
      <li>
        Online cooking school – registration, content delivery, data management.
      </li>
      <li>
        Various custom ecommerce applications (music, digital documents, video subscriptions, etc.)
      </li>
    </ul>

    <h3>
      Quick facts
    </h3>
    <ul>
      <li>
        Location: Comox, Vancouver Island, BC, Canada
      </li>
      <li>
        Languages: English and German
      </li>
      <li>
        Timezone: Pacific Time
        <span id="current_time"></span>
      </li>
      <li>
        Founded: 2006
      </li>
    </ul>

  </div>

  <div class="span5 offset1">
    <h3>
      Tools
    </h3>
    <p>
      Below is a list of our current technology preferences:
    </p>
    <p>Front end:</p>
    <ul>
      <li>ClojureScript/re-frame/react</li>
      <li>HTML/CSS/Javascript</li>
      <li>React Native</li>
    </ul>
    <p>Server:</p>
    <ul>
      <li>Ruby and Ruby on Rails/Puma/Sidekiq</li>
      <li>Nginx</li>
      <li>Nodejs</li>
      <li>Clojure</li>
      <li>Linux</li>
    </ul>
    <p>Systems integration:</p>
    <ul>
      <li>Payment processing</li>
      <li>Email sending</li>
      <li>Server monitoring</li>
      <li>Social media APIs</li>
      <li>PDF creation</li>
      <li>RSS and ATOM feeds</li>
      <li>OAuth</li>
      <li>Analytics</li>
      <li>Custom APIs</li>
    </ul>
    <p>Data:</p>
    <ul>
      <li>PostgreSQL</li>
      <li>Redis</li>
      <li>Elasticsearch</li>
      <li>Lucene</li>
      <li>git</li>
      <li>JSON/XML</li>
    </ul>
    <p>Capabilities:</p>
    <ul>
      <li>Functional programming, object oriented programming.</li>
      <li>Parsing</li>
      <li>Natural language processing</li>
      <li>Machine learning</li>
      <li>Regular expressions</li>
      <li>Internationalization: Unicode, localization, utf8, timezones, fonts.</li>
      <li>Dynamic programming</li>
      <li>Neural networks</li>
      <li>Diff-match-patch</li>
    </ul>
    <p>Tools:</p>
    <ul>
      <li>Sublime Text</li>
      <li>git</li>
      <li>Command line terminal</li>
      <li>macOS</li>
      <li>Whiteboard/hammock/gardening/running</li>
    </ul>
  </div>
</div>
<script src="/script/moment.min.js"></script>
<script src="/script/moment-timezone-with-data.min.js"></script>
<script>
  function renderCurTime() {
    document.getElementById("current_time").innerHTML=(
      "(" + 
      moment().tz('America/Vancouver').format('h:mm a') +
      ")"
    ); 
  };
  renderCurTime();
  setInterval(renderCurTime, 30000);
</script>
