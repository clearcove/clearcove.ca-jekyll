---
title: 'Recipe: Make request environment available to models in Rails'
author: jhund
layout: post
permalink: /2008/08/recipe-make-request-environment-available-to-models-in-rails/
categories:
  - Ruby/Rails
tags:
  - broadcast
---
How often do you wish you had access to current_user in one of your models? I needed it for an app that required auditing. I had ActiveRecord call backs on the audited models to create audit entries on every record operation. The problem was that the models had no access to the currently logged in user. <!--more-->

## Objective

To store web request environment parameters accessibly everywhere in my Rails app. Examples:

  * the current user needs to be accessible to my Models to implement an audit trail via Active Record callbacks.
  * I want to store the current time zone so that it can be used by the various parts of my app (This is actually what Rails does for TimeZone support).

You might say &#8220;Auditing can be done in the controller&#8221;. True, however I find it much more elegant to put it into the models. So how did I do it? I used ActiveRecord call backs on creating, updating, and deleting records. I stored the current user in a way that the models can access it. The same way Rails stores the current time zone for a request: In the current thread. Check out the [rails source for details how it is done there.][1]

## Summary

  * implement accessors for &#8216;current&#8217; on each class that is a request environment parameter. In this example I implement User.current
  * set the variable via the accessors in an application\_controller before\_filter
  * accessors store current request parameters in Thread.current[:parameter_name]

Please see [github][2] for the code for this and my other recipes.

Here is the code to store a variable in the current thread so that it is accessible everywhere in the Rails app:

In application controller

<pre class="brush: ruby; title: ; notranslate" title="">class ApplicationController &lt; ActionController::Base

  before_filter :set_request_environment

private

  # stores parameters for current request
  def set_request_environment
    User.current = current_user # current_user is set by restful_authentication
    # You would also set the time zone for Rails time zone support here:
    # Time.zone = Person.current.time_zone
  end

end
</pre>

In your User model

<pre class="brush: ruby; title: ; notranslate" title="">class User &lt; ActiveRecord::Base

  #-----------------------------------------------------------------------------------------------------
  # CLASS METHODS
  #-----------------------------------------------------------------------------------------------------

  def self.current
    Thread.current[:user]
  end

  def self.current=(user)
    raise(ArgumentError,
        "Invalid user. Expected an object of class 'User', got #{user.inspect}") unless user.is_a?(User)
    Thread.current[:user] = user
  end

end
</pre>

There is a danger of abusing this for global variables. They are evil. However I think it is perfectly valid to make the current time zone, the current user, and possibly any other scoping resource globally available. They are like gravity &mdash; they&#8217;re everywhere, completely pervasive.

And the nice thing is that this also works on the console. Before I manipulate data, I can set Person.current to a user, and will have an audit trail for things I did outside of the running rails app.

 [1]: http://github.com/rails/rails/tree/master/activesupport/lib/active_support/core_ext/time/zones.rb
 [2]: http://github.com/jhund/rails-recipes/tree/master