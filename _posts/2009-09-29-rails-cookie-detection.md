---
title: 'Recipe: Detect whether cookies are enabled in rails'
author: jhund
layout: post
permalink: /2009/09/rails-cookie-detection/
other_media:
  -
main_video:
  -
categories:
  - Ruby/Rails
---
If your Rails app requires cookies, this recipe is for you: It detects whether cookies are enabled, and if not, shows a message to your users.

<!--more-->

This recipe checks for the presence of a specific cookie (&#8220;cookie\_test&#8221;). If it is present, the request continues normally. If it is not present, then it sets the cookie, redirects to the &#8220;cookie\_test&#8221; action and checks there whether the cookie is present. If the cookie is present, then everything continues on normally. If the cookie is still not present, then this is a strong indication that the user has cookies disabled. Then the recipe shows a page to the user explaining the cookie requirement. I leave it up to you to write the copy for that page.

<div id="attachment_419" style="width: 358px" class="wp-caption alignnone">
  <img src="https://clearcove.ca/images/2009/09/CookieDetectionFlowChart.png" alt="Flow chart for the cookie detection recipe" title="CookieDetectionFlowChart" width="348" height="460" class="size-full wp-image-419" />

  <p class="wp-caption-text">
    Flow chart for the cookie detection recipe
  </p>
</div>

This recipe works as of Rails 2.3 (new cookie handling). It is inspired by this [article][1]. The recipe in the original article does not work with newer versions of rails. So I updated it and packaged it up in a neat little module that you can include in your application controller.

## How to use it

  1. save this file at a location in your rails app where it gets required (e.g. RAILS\_ROOT/lib/cookie\_detection.rb)
  2. In application_controller.rb:
    Include CookieDetection (at the top to make this the first before_filter)
  3. In routes.rb:
    map.cookies\_test &#8220;cookie\_test&#8221;, :controller => &#8220;application&#8221;, :action => &#8220;cookie_test&#8221;
  4. In RAILS\_ROOT/views/shared/cookies\_required.html.erb:
    Display a message that lets your users know of the cookie requirement.

<pre class="brush: ruby; title: ; notranslate" title=""># RAILS_ROOT/lib/cookie_detection.rb

# Detects if cookies are present.
# If cookies are disabled, shows view located at shared/cookies_required
# Usage:
# * save this file at a location in your rails app where it gets required
#   (e.g. RAILS_ROOT/lib)
# * application_controller:
#   Include CookieDetection (at the top to make this the first before_filter)
# * routes.rb:
#   map.cookies_test "cookie_test", :controller =&gt; "application", :action =&gt; "cookie_test"
# * views/shared/cookies_required.html.erb:
#   Display a message that lets your users know of the cookie requirement.
module CookieDetection

  def self.included(base)
    base.before_filter :cookies_required, :except =&gt; ["cookie_test"]
  end

  # checks for presence of "cookie_test" cookie
  # (should have been set by cookies_required before_filter)
  # if cookie is present, continue normal operation
  # otherwise show cookie warning at "shared/cookies_required"
  def cookie_test
    if cookies["cookie_test"].blank?
      logger.warn("=== cookies are disabled")
      render :template =&gt; "shared/cookies_required", :layout =&gt; "system"
    else
      redirect_back_or_default(dashboard_path)
    end
  end

protected

  # checks for presence of "cookie_test" cookie.
  # If not present, redirects to cookies_test action
  def cookies_required
    return true unless cookies["cookie_test"].blank?
    cookies["cookie_test"] = Time.now
    session[:return_to] = request.request_uri
    redirect_to(cookies_test_path)
  end

end
</pre>

 [1]: http://kill-0.com/duplo/2007/07/12/rails-cookie-detection/
