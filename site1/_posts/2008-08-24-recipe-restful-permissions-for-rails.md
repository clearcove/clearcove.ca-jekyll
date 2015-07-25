---
title: 'Recipe: RESTful permissions for Rails'
author: jhund
layout: post
permalink: /2008/08/recipe-restful-permissions-for-rails/
categories:
  - Ruby/Rails
tags:
  - broadcast
---
A common requirement for Rails applications is to check permissions for certain actions on your RESTful application&#8217;s resources. There are many ways to solve this problem, ranging from simple boolean flags to full fledged [role based access control][1]. I have tried a lot of approaches and have settled on a fairly simple way that is RESTful and quite flexible.

<!--more-->

## Updates

  * 20081023 &#8211; see the code for this and other recipes at [github][2]
  * 20081018 &#8211; changed syntax highlighter, replaced Person with User

## Scope

What it does and what it does not: We are just controlling access to our app&#8217;s resources at the view and controller level, i.e. access through the web. We do not check permissions on a model or file level. That means anybody with access to the server can mess with your files, database, and models.

I consider this approach the sweet spot for most of my applications which don&#8217;t contain super sensitive information.

## Overview

There are a number of places where one needs to check for permissions in a rails app:

  * in the **View** when deciding which controls a user should see: Should we display the edit button on that resource? Is this user allowed to delete this record? This approach renders UI elements only for those actions the current user has permission for.
  * in the **Controller** when protecting certain URLs. Even if you don&#8217;t provide a link to access a resource in the view, it would be easy to guess the RESTful URL for certain operations. So every controller action checks permissions for the current user before any data is modified.
  * if you want it really secure, you also have to protect your **Model**. I might write about this later. It is beyond the scope of this article. Quick pointer: you could use ActiveRecord callbacks and raise exceptions if `current_user` does not have permissions.

One issue that comes up when checking permissions is &#8220;How do models get access to `current_user`?&#8221;. This is important for auditing and model security. I copied what Rails is doing: store the current user in `Thread.current[:user]` at the beginning of each request and wrap User.current around that variable. Then User.current is available everywhere in your app, not only in controllers and views.

## Recipe

The following is the recipe for implementing RESTful permissions in an *Online Syllabus Builder* Rails app:

### User Model

There isn&#8217;t much you need on the user model. I assume you are using [restful_authentication][3] to handle users, signups, and logins. All I usually do these days is to add a boolean field for each role a user can have.

In our example the roles are &#8216;is\_admin&#8217; and &#8216;is\_editor&#8217;. For each role a user holds, the corresponding field is set to &#8216;true&#8217;. You can then ask the user: current\_user.is\_admin? when checking for permissions.

### Resource Model

Tell your model that it has permissions and customize any special permissions:

In `app/models/syllabus.rb`

<pre class="brush: ruby; title: ; notranslate" title="">class Syllabus &lt; ActiveRecord::Base

  has_restful_permissions

  ...

  def updatable_by?(actor)
    actor.is_a?(User) && (actor.is_admin? || self.owned_by?(actor))
  end

  def owned_by?(actor)
    actor == self.author
  end

end
</pre>

The statement `has_restful_permissions` includes the standard set of permissions (see bottom of this post for an example). You will find that in a RESTful app, most resources have the same rules for permissions, so you can extract the permission rules into a module and include it into your resources. Then you can override individual rules for resources that are different.

The method `updatable_by?` overrides the default permissions for the update action on Syllabus. In this case it grants permission to update a Syllabus to logged in users (`actor.is_a?(User)`) who are either admins (`actor.is_admin?`) or who own this syllabus (`self.owned_by?(actor)`).

The method `owned_by?` defines who owns this resource. This is the method you will have to override most often as there is no general solution that fits all. In this case we consider the author of a syllabus its owner. For nested resources, you can call `owned_by?` on the parent resource. Example:

In `app/models/text_block.rb`

<pre class="brush: ruby; title: ; notranslate" title="">class TextBlock &lt; ActiveRecord::Base

  has_restful_permissions
  belongs_to :syllabus

  def owned_by?(actor)
    self.syllabus.owned_by?(actor)
  end

end
</pre>

### View

Whenever you have a resource action link that is rendered based on the current user&#8217;s permissions:

In `app/views/syllabi/index.html.erb<br />
`

<pre class="brush: ruby; title: ; notranslate" title="">&lt;%= link_to('Edit', edit_syllabus_path(@syllabus)) if @syllabus.updatable_by?(current_user) %&gt;
</pre>

The &#8216;Edit&#8217; link is rendered only if the current_user has permission to update this syllabus.

There is a slight edge case when rendering links for creating a new resource. Because you don&#8217;t have the resource instantiated when the view is rendered, you might have to call creatable_by? on the resource class instead of an instance. So `Syllabus.creatable_by?` instead of `@syllabus.creatable_by?`

### Controller

The remaining place to check permissions is in your controller actions. I usually instantiate the resource first and then check permissions so that I can consider ownership associations as well as any other permissions that depend on the state of the resource:

In `app/controllers/syllabi_controller.rb<br />
`

<pre class="brush: ruby; title: ; notranslate" title="">class SyllabiController &lt; ApplicationController

  def index
    # Note that this is the only case where we check permissions on the class
    raise PermissionViolation unless Syllabus.listable_by?(current_user)
    @syllabi = ...
    ...
  end

  def show
    @syllabus = Syllabus.find(params[:id])
    raise PermissionViolation unless @syllabus.updatable_by?(current_user)
    ...
  end

  def new
    @syllabus = Syllabus.new
    raise PermissionViolation unless @syllabus.creatable_by?(current_user)
    ...
  end

  def edit
    @syllabus = Syllabus.find(params[:id])
    raise PermissionViolation unless @syllabus.updatable_by?(current_user)
    ...
  end

  def create
    @syllabus = Syllabus.new(params[:syllabus])
    raise PermissionViolation unless @syllabus.creatable_by?(current_user)
    ...
  end

  def update
    @syllabus = Syllabus.find(params[:id])
    raise PermissionViolation unless @syllabus.updatable_by?(current_user)
    ...
  end

  def destroy
    @syllabus = Syllabus.find(params[:id])
    raise PermissionViolation unless @syllabus.destroyable_by?(current_user)
    ...
  end
end
</pre>

I first instantiate the current resource and then I ask it whether the current_user has permission to perform the requested action. This assumes that instantiating the resource has no side effects.

You can handle a `PermissionViolation` in the application controller.

In `app/controllers/application.rb`

<pre class="brush: ruby; title: ; notranslate" title="">class ApplicationController &lt; ActionController::Base

  def rescue_action(exception)
    case exception
      when PermissionViolation
        flash[:warning] = "You do not have permission for this action."
        redirect_to :back
      else
        super
    end
  end

end
</pre>

### Permission types

This list contains the most common permissions for a RESTful resource. You might need additional permissions for each custom action method you add in `routes.rb` to member or collection:

  * show: `@syllabus.viewable_by?(current_user)`
  * edit and update: `@syllabus.updatable_by?(current_user)`
  * new and create: `@syllabus.creatable_by?(current_user)` — your initial instinct might be to make this a class method, however I found it useful to instantiate the new object, then check permissions before I actually save it. Allows you to consider associated resources for permission checks. Sometimes you need to call creatable_by? on the class. For example when rendering a &#8216;new&#8217; link in a view that allows you to create a new resource, however you don&#8217;t have one instantiated at that point to check permissions on.
  * destroy: `@syllabus.destroyable_by?(current_user)`
  * index: `Syllabus.listable_by?(current_user)` — for list I use a class method because there is no single resource for lists.

So what do these methods look like? Below are some example methods that you would define in your resource class:

Anybody can list syllabi:

<pre class="brush: ruby; title: ; notranslate" title="">def self.listable_by?(actor)
  true
end
</pre>

Logged in users can view this syllabus:

<pre class="brush: ruby; title: ; notranslate" title="">def viewable_by?(actor)
  actor.is_a?(User)
end
</pre>

Admins and owners can update this post:

<pre class="brush: ruby; title: ; notranslate" title="">def updatable_by?(actor)
  actor.is_a?(User) && (actor.is_admin? || self.owned_by?(actor))
end
</pre>

Nobody can destroy this post:

<pre class="brush: ruby; title: ; notranslate" title="">def destroyable_by?(actor)
  false
end
</pre>

## Other implementations

### Nick Kallen&#8217;s post

I found an [article by Nick Kallen][4] that proposed a very similar approach a while ago.

He uses active voice: `current_user.can_create(@syllabus)`. That works if you have an AnonymousUser object. And you also need to do the double dispatch where the user object then asks the resource whether it has permission. I find using the passive voice directly more concise &mdash; even though in general I prefer active voice. It is much less ambiguous.

One aspect Nick does not mention is the concept of resource ownership, handled by &#8220;`owned_by?(actor)`&#8221; in this example.

Also [Hobo][5] and [spot.us][6], written by Hashrocket use a similar approach.

### make_resourceful

This recipe integrates very nicely with the [make_resourceful][7] plugin. All you need to do is call

<pre class="brush: ruby; title: ; notranslate" title="">raise PermissionViolation unless current_object.viewable_by?(current_user)
</pre>

in the before_show callback. And then the resources take care of answering that question. I actually modified the plugin and added permissions checks in `/vendor/plugins/make_resourceful/lib/resourceful/default/actions.rb`:

<pre class="brush: ruby; title: ; notranslate" title="">module Actions
  # GET /foos
  def index
    #load_objects
    raise PermissionViolation unless current_model.listable_by?(User.current)
    before :index
    response_for :index
  end
  ...
end
</pre>

### Confused deputy problem

Something to keep in mind when working with permissions is what is called the &#8220;[confused deputy problem][8]&#8220;. Where the app intends to give a particular permission to an actor. The actor then does something either inadvertently or on purpose that goes beyond the permission granted. This usually applies to file system resources and file permissions. However it can also happen in Rails through model associations. A user might have permission to delete resource A. Resource A `has_many :bs, dependent => :destroy`. When the user deletes resource A, Rails automatically also deletes resource B &mdash; which the user might not have permission for. You would need model based permissions to prevent this from happening.

## Code for default permissions

Here is the code I usually include in resources that have permissions with &#8220;`has_restful_permissions`&#8221; in the class definition.

Require it with

<pre>require 'has_restful_permissions'</pre>

In `lib/has_restful_permissions.rb`

<pre class="brush: ruby; title: ; notranslate" title="">class PermissionViolation &lt; StandardError; end

module HasRestfulPermissions

  # call this in resource class
  def has_restful_permissions
    extend  ClassMethods
    include InstanceMethods
  end

  module InstanceMethods

    # permission rules, override these in the resource class

    # Returns true if actor can create this new instance.
    # I prefer the instance method over the class method
    # because sometimes you need to look at the state
    # of the newly instantiated object or its related objects
    # to decide whether the current user is permitted to create it.
    def creatable_by?(actor)
      actor.is_a?(User)
    end

    # Returns true if actor can destroy this resource.
    def destroyable_by?(actor)
      actor.is_a?(User) && self.owned_by?(actor)
    end

    # Returns true if actor can update this resource.
    def updatable_by?(actor)
      actor.is_a?(User) && self.owned_by?(actor)
    end

    # Returns true if actor can view this resource.
    def viewable_by?(actor)
      actor.is_a?(User)
    end

    # Returns true if actor owns this resource.
    # Expresses a control/ownership association.
    # Expects actor to be a User (usually checked for in other
    # permission methods before this method is called)
    def owned_by?(actor)
      # defaults to false. Override if resource has owner
      false
    end

  end

  module ClassMethods
    # Returns true if actor can view a list of resources of this class.
    def listable_by?(actor)
      actor.is_a?(User)
    end

    # alternative way to check if resource is creatable by actor.
    # use this class method instead of the instance method if resource
    # is not instantiated at time of checking permissions
    # This is useful when deciding whether to render a "new" link
    # used for creating a new resource.
    def creatable_by?(actor)
      actor.is_a?(User)
    end
  end

end

ActiveRecord::Base.send :extend, HasRestfulPermissions
</pre>

 [1]: http://www.railsforum.com/viewtopic.php?id=14216
 [2]: http://github.com/jhund/rails-recipes/tree/master
 [3]: http://github.com/technoweenie/restful-authentication
 [4]: http://pivots.pivotallabs.com/users/nick/blog/articles/272-access-control-permissions-in-rails
 [5]: http://hobocentral.net/
 [6]: http://github.com/spot-us/spot-us/tree/master
 [7]: http://mr.hamptoncatlin.com/
 [8]: http://en.wikipedia.org/wiki/Confused_deputy_problem