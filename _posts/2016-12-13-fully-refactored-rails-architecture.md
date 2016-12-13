---
title: Fully refactored Ruby on Rails architecture
layout: post
permalink: /2016/12/fully-refactored-rails-architecture
---

The linked article provides a very thorough walk-through of a fully refactored Rails app using the example of a running tracker.

When would you want a fully refactored Rails app? I’ve written about this earlier in [How to change objects](http://rails-recipes.clearcove.ca/pages/how_to_change_objects.html).

Make sure to read the entire article: [Build Sleek Rails Components With Plain Old Ruby Objects](https://www.toptal.com/ruby-on-rails/decoupling-rails-components).

Here is a quick summary of the main points:

To achieve the new design, I’ll use the guidelines listed below, but please note these are not rules you have to follow to the T. Think of them as flexible guidelines that make refactoring easier.

* **ActiveRecord models can contain associations and constants, but nothing else**. So that means no callbacks (use service objects and add the callbacks there) and no validations (use Form objects to include naming and validations for the model).
* **Keep Controllers as thin layers and always call Service objects**. Some of you would ask why use controllers at all since we want to keep calling service objects to contain the logic? Well, controllers are a good place to have the HTTP routing, parameters parsing, authentication, content negotiation, calling the right service or editor object, exception catching, response formatting, and returning the right HTTP status code.
* **Services should call Query objects, and should not store state**. Use instance methods, not class methods. There should be very few public methods in keeping with SRP.
* **Queries should be done in query objects**. Query object methods should return an object, a hash or an array, not an ActiveRecord association.
* **Avoid using Helpers and use decorators instead**. Why? A common pitfall with Rails helpers is that they can turn into a big pile of non-OO functions, all sharing a namespace and stepping on each other. But much worse is that there’s no great way to use any kind of polymorphism with Rails helpers — providing different implementations for different contexts or types, over-riding or sub-classing helpers. I think the Rails helper classes should generally be used for utility methods, not for specific use cases, such as formatting model attributes for any kind of presentation logic. Keep them light and breezy.
* **Avoid using concerns and use Decorators/Delegators instead**. Why? After all, concerns seem to be a core part of Rails and can DRY up code when shared among multiple models. Nonetheless, the main issue is that concerns don’t make the model object more cohesive. The code is just better organized. In other words, there’s no real change to the API of the model.
* **Try to extract Value Objects** from models to keep your code cleaner and to group related attributes.
* **Always pass one instance variable per view**.
