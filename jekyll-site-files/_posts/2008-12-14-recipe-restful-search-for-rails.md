---
title: 'Recipe: RESTful search for Rails'
author: jhund
layout: post
permalink: /2008/12/recipe-restful-search-for-rails/
main_video:
  -
other_media:
  -
categories:
  - Ruby/Rails
tags:
  - broadcast
---
This recipe shows you how to search, filter, and sort your resource lists in a restful way. We will look at the most simple way to accomplish this and then provide some pointers to further improvements. This recipe works great with will_paginate. It is an end to end solution (model, view, and controller). This recipe requires Rails 2.2.2 or greater if you want to use :joins in named scopes. Otherwise Rails 2.0 is sufficient.

<!--more-->

Here is what you will get from this recipe:

<div id="attachment_369" style="width: 310px" class="wp-caption alignnone">
  <a href="http://clearcove.ca/images/2008/12/quentinlistoptions.jpg"><img src="http://clearcove.ca/images/2008/12/quentinlistoptions-300x96.jpg" alt="List options in the Quentin time tracker" title="quentin_list_options" width="300" height="96" class="size-medium wp-image-369" /></a>

  <p class="wp-caption-text">
    List options in the Quentin time tracker
  </p>
</div>

## Overview

Named scopes are an important ingredient to this recipe. I consider them one of the most useful features that have been added to Rails lately. Key is to name them properly and then you can use the scopes&#8217; names all the way from the view, via controller, to the model. Super convenient and clean.

Here is what we do:

### Model

  * Define a named scope for every search/filter dimension
  * Define a method called &#8216;filter&#8217; to retrieve the records from the DB
  * Define a method called &#8216;list\_option\_names&#8217; to get a list of possible list options

### Controller

  * In the index action, call the Activity.filter method instead of Activity.find
  * Add a private method &#8216;load\_list\_options&#8217; to extract the options from the request

### View

  * Add a form to your resource list to define the list options
  * Render the resources as list

## Example code

As an example application we use a time tracker (See <a href="http://github.com/jhund/quentin" alt="Quentin on github">Quentin</a> on github). The resource we want to search on are Activities. Every task has a number of Activities for time worked on this task. And each Activity belongs to a Client and Project.

You can look at the code below or go straight to <a href="http://github.com/jhund/rails-recipes" alt="Quentin on github">my Rails recipes on github</a> where I have a sparse Rails app with all the bits and pieces required for RESTful searching in Rails.

### Model

First of all let&#8217;s define some named scopes that we can use for our searching:

<pre class="brush: ruby; title: ; notranslate" title=""># In app/models/activity.rb

named_scope :search, lambda{ |query|
    q = "%#{query.downcase}%"
    { :conditions =&gt; ['lower(activities.description) LIKE ?', q] } }
named_scope :for_project, lambda { |project_ids|
    { :conditions =&gt; ['activities.project_id IN (?)', [*project_ids]] } }
# don't call the below 'sort_by'. There seems to be some naming conflict
named_scope :sorted_by, lambda { |sort_option|
    case sort_option.to_s
    when 'most_recent'
      { :order =&gt; 'activities.created_at DESC' }
    when 'client_project_name_a_to_z'
      { :order =&gt; 'lower(clients.name) ASC, lower(projects.name) ASC',
        :joins =&gt; {:project =&gt; :client} }
    when 'duration_shortest_first'
      { :order =&gt; 'activities.duration ASC' }
    when 'duration_longest_first'
      { :order =&gt; 'activities.duration DESC' }
    else
      raise(ArgumentError, "Invalid sort option: #{sort_option.inspect}")
    end }
</pre>

All of my searchable resources have the &#8216;search&#8217; and the &#8216;sorted\_by&#8217; named scope. They are used for indexed searching (in this case just using the database LIKE method), and sorting. Starting with Rails 2.2 you can also use :joins in named\_scopes to extend your searches across multiple tables.

The :for\_project named scope accepts project\_ids either as array or as single integer. The [*project_ids] converts either into an array of params for the SQL &#8216;IN&#8217; clause.

Then we need to define the filter method that composes the query and retrieves the records from the database. This is a class method on Activity:

<pre class="brush: ruby; title: ; notranslate" title=""># In app/models/activity.rb

# applies list options to retrieve matching records from database
def self.filter(list_options)
  raise(ArgumentError, "Expected Hash, got #{list_options.inspect}") \
      unless list_options.is_a?(Hash)
  # compose all filters on AR Collection Proxy
  ar_proxy = Activity
  list_options.each do |key, value|
    next unless self.list_option_names.include?(key) # only consider list options
    next if value.blank? # ignore blank list options
    ar_proxy = ar_proxy.send(key, value) # compose this option
  end
  ar_proxy # return the ActiveRecord proxy object
end
</pre>

The method above is where the magic happens. The really cool thing about named scopes is that they are [composable][1]. This is a really powerful concept where we chain together all the conditions that were defined in the resource filter form. Rails will defer the single DB query with all the conditions until it actually needs the collection. This is what makes this approach so efficient.

The &#8216;list\_option\_names&#8217; method returns the names of the list options we use. Again, we use a cool feature of named scopes where we can reflect on the defined named scopes and just remove the ones that are not part of the list options.

<pre class="brush: ruby; title: ; notranslate" title=""># In app/models/activity.rb

# returns array of valid list option names
def self.list_option_names
  self.scopes.map{|s| s.first} - [:named_scope_that_is_not_a_list_option]
end
</pre>

### Controller

The controller has two jobs in this recipe:

  * Get the list options from the submitted resource search form
  * Make the current list options accessible to the resource search form for display

Define the following methods in the activities controller:

We use the index option to render the search results. It behaves like a normal index action if no list_options are given. If they are given, they will be considered when retrieving records from the Database.

<pre class="brush: ruby; title: ; notranslate" title=""># In app/controllers/activities_controller.rb

def index
  @list_options = load_list_options
  @activities = Activity.filter(@list_options).paginate(:page =&gt; params[:page])
end
</pre>

Of course you can add a response block and render the results to HTML, XML, CSV, etc. Notice how I use will_paginate exactly like I would use it with a normal Activity.find.

The &#8216;load\_list\_options&#8217; method builds the hash of list options for this request:

<pre class="brush: ruby; title: ; notranslate" title=""># In app/controllers/activities_controller.rb

private

def load_list_options
  # define default list options here. They will be used if none are given
  options = {:sorted_by =&gt; 'most_recent'}
  # find relevant query parameters and override list options
  Activity.list_option_names.each do |name|
    options[name] = params[name] unless params[name].blank?
  end
  options
end
</pre>

We use the list of list\_option\_names to extract the relevant params and store them in an instance variable so that they are available to the view.

### View

Finally we get to see the results of all this work:

<pre class="brush: xml; title: ; notranslate" title=""># In app/views/activities/index.html.erb

&lt;h1&gt;Activities&lt;/h1&gt;
&lt;!-- &lt;pre&gt;&lt;%#= @list_options.to_yaml %&gt;&lt;/pre&gt; --&gt;

&lt;div id="list_options"&gt;
  &lt;% form_tag(activities_path, :method =&gt; :get) do %&gt;
    &lt;dl&gt;
      &lt;dt&gt;Search&lt;/dt&gt;
      &lt;dd&gt;&lt;%= text_field_tag('search', @list_options[:search]) %&gt;&lt;/dd&gt;
      &lt;dt&gt;Project&lt;/dt&gt;
      &lt;dd&gt;
        &lt;%= select_tag('for_project', options_for_select(
          Project.all_with_client.sorted_by('client_and_name_a_to_z').map{|p|
              [p.client_and_name, p.id.to_s]}.unshift(['- Any -', nil]),
          @list_options[:for_project])) %&gt;
      &lt;/dd&gt;
      &lt;dt&gt;Sorted by&lt;/dt&gt;
      &lt;dd&gt;
        &lt;%= select_tag('sorted_by',options_for_select(
          [
            ['most recent', 'most_recent'],
            ['client, project name (a-z)', 'client_project_name_a_to_z'],
            ['duration (longest first)', 'duration_longest_first'],
            ['duration (shortest first)', 'duration_shortest_first']
          ],
          @list_options[:sorted_by])) %&gt;
      &lt;/dd&gt;
      &lt;dt&gt;&lt;/dt&gt;
      &lt;dd&gt;
        &lt;%= submit_tag "Update list" %&gt; or
        &lt;%= link_to('Reset', activities_path) %&gt;
      &lt;/dd&gt;
    &lt;/dl&gt;
  &lt;% end %&gt;
&lt;/div&gt;

&lt;div class="pagination_group"&gt;
  &lt;%= will_paginate(@activities) %&gt;
  &lt;%= page_entries_info(@activities) %&gt;
&lt;/div&gt;

&lt;div&gt;
  &lt;table&gt;
    &lt;tr&gt;
      &lt;th&gt;Client | Project&lt;/th&gt;
      &lt;th&gt;Description&lt;/th&gt;
      &lt;th&gt;Duration&lt;/th&gt;
      &lt;th&gt;Date&lt;/th&gt;
    &lt;/tr&gt;
    &lt;% @activities.each do |activity| %&gt;
      &lt;%= render :partial =&gt; activity %&gt;
    &lt;% end %&gt;
  &lt;/table&gt;
&lt;/div&gt;
</pre>

A few comments on the view:

  * Near the top is some commented out code that is helpful for debugging list options.
  * The form submits to the standard route for the activities index action via the GET method. This has the advantage that you can bookmark a particular search and link to it directly with all the search params in the URL.
  * The search inputs are standard form fields that get their default values from the @list_options hash. So you always see what the settings for the current search are.
  * For the &#8216;Project&#8217; select input, I add the &#8216;Any&#8217; option with a nil value. The &#8216;Any&#8217; option will be ignored when composing the AR conditions because the value is nil. This is exactly what we want: If we don&#8217;t want a particular client, then the named scope &#8216;with_client&#8217; should not be used.
  * Also on the client select, note that I typecast the id to string. This is important to show the correct selected option, because all @list_options params are in string format from the request params.
  * Finally the &#8216;Reset&#8217; link just points to the index action with no params given. In this case the default list options will be used.
  * I added basic code for pagination. See [will_paginate documentation][2] for more info.
  * Finally the list of resources is rendered using a partial.

## Additional features

  * Sometimes it makes sense to persist the list options in the session so the list remembers my settings when I come back to it during the same session. The place to do this is in the controller&#8217;s &#8216;load\_list\_options&#8217; method.
  * You could Ajaxify this quite easily; I decided not to show it to make the essential stuff more visible.
  * Add preset links for common filter settings: <pre class="brush: xml; title: ; notranslate" title="">link_to('Longest phone calls', activities_path(:search =&gt; 'call', :sorted_by =&gt; 'duration_longest_first'))</pre>

  * Namespace the list option params. Form input fields would be &#8216;list\_options[sorted\_by]&#8216; instead of &#8216;sorted_by&#8217;. This might be necessary if you have a more complex app where there might be parameter name conflicts.
  * Make @list\_options an AR object so you can use the &#8216;form\_for(&#8230;) do |f|&#8217; form, validations, and datetime type casts in your filter forms. You can also save searches for later use.
  * Use a form builder to build the filter control inputs. This way you could minimize the filter form portion of your view code (example: f.sort\_by\_select).
  * I haven&#8217;t tried it, but you might be able to use subqueries in your named_scopes as well. See [this article][3] for more info.

## Other resources

  * Railscasts [episode 37][4] (full stack solution, doesn&#8217;t use named scopes)
  * <a jref="http://www.bencurtis.com/archives/2008/06/restful-searching-in-rails/">Ben Curtis</a> (full stack solution, doesn&#8217;t use named scopes)
  * [ Courtenay][5] (uses named_scopes, puts stuff in the controller that doesn&#8217;t belong there, thanks for the tip on the :joins bug)
  * [TechnoWeenie][6] (not a full stack solution. Provides convenience named_scopes for AR models)

 [1]: http://en.wikipedia.org/wiki/Composability
 [2]: http://github.com/mislav/will_paginate
 [3]: http://pivotallabs.com/users/jsusser/blog/articles/567-hacking-a-subselect-in-activerecord
 [4]: http://railscasts.com/episodes/37
 [5]: http://www.caboo.se/articles/2008/8/26/the-awesomest-filter-and-sort-ever
 [6]: http://github.com/technoweenie/can_search/tree/master
