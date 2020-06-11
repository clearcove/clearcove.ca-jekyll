---
title: ActiveInteraction with dry-validation
layout: post
permalink: /2020/05/active-interaction-with-dry-validation
---

The [ActiveInteraction](https://github.com/AaronLasseigne/active_interaction) gem is super useful, however we like to use [dry-validation](https://github.com/dry-rb/dry-validation) for input argument validation.

Below is a sketch for how to integrate ActiveInteraction, dry-validation, and [dry-container](https://dry-rb.org/gems/dry-container/0.8/) to build interactions with the following features:

* Implement the `Command` pattern in Ruby on Rails to make processes and interactions first class citizens, keeping Controllers and Models very simple.
* Type cast and validate all interaction inputs.
* Share and compose schema validations between interactions.
* Handle outcomes and error reporting.
* Compose interactions to build complex workflows.
* Interactions are invoked in the same way from controllers, background jobs, production consoles, or other interactions.
* Support dependency injection to facilitate testing.

# `Data` vs. `Behavior`

At a high level, we distinguish these two concerns to help us structure our Rails application:

## Data

All data related concerns live in `models`:

* Attributes (DB columns)
* Association declarations
* Some validations
* ActiveRecord scopes
* Method delegation
* NOTE: There should be no callbacks in models. They fall clearly in the domain of `behavior`.

and `contracts`:

* Schema specification for a given data structure.
* Higher level (business logic) validation rules.

NOTE: contracts can also live directly inside an interaction if they are not shared with other interactions.

## Behavior

All behavior related concerns live in `interactions`:

* Any mutation of database records.
* Multi-step and nested processes.
* Interactions with external services.

# Workspaces

Another architectural choice we make is to group all behavior and ui related code into workspaces. Workspaces should be aligned with stable concepts in the application domain, e.g., user roles, departments, or stable software architectural concepts. Some examples:

* `authentication` (log-in, password resets, session management)
* `admin` (administrator tasks like user management, lookup values management)
* `system` (hardware reports, background jobs UI)
* `finance` (UI for finance department)
* `anonymous` (Public faces that don't require authentication)
* `teachers` (UI for users with a teacher role)

# Installation

Add these gems to your Gemfile:

```
gem "active_interaction"
gem "dry-container"
gem "dry-validation"
```

Add these folders to your Rails application:

```
* app
    * contracts
    * interactions
```

# Usage

Below is an example for an interaction used to create issues:

The interaction is invoked from a controller:

```ruby
# app/controllers/comms/issues_controller.rb
...

def create
  iao = Comms::Issues::Create.run(
    args: params
            .fetch(:issue, {})
            .to_unsafe_h
            .merge(reporter_id: current_user.id),
  )
  respond_to do |format|
    format.html {
      if iao.valid?
        @issue = iao.result
        redirect_to comms_issue_path(@issue)
      else
        @issue = iao
        render(:new)
      end
    }
  end
end

...
```

This is what the interaction looks like:

```ruby
# apps/interactions/comms/issues/create.rb
module Comms
  module Issues
    # Creates an Issue
    class Create < ActiveInteraction::Base

      include ActiveInteractionWithDryValidation

      # Contract for validating args
      # NOTE: This could also inherit from a shared contract like so:
      #     class ArgsContract < Comms::Issues::CreateContract; end
      #     (shared contract is at app/contracts/comms/issues/create_contract.rb)
      class ArgsContract < Dry::Validation::Contract

        params do
          required(:title).filled(:str?)
          optional(:description).maybe(:str?)
          required(:reporter_id).value(format?: UUID_REGEX)
          optional(:assignee_id).maybe(format?: UUID_REGEX)
          required(:issue_type).value(:str?)
        end

      end

      # @return [Issue] the newly created Issue
      def execute
        issue = Issue.new(args.to_h)
        if issue.save
          # Add a comment to the issue timeline
          compose(
            Comms::IssueComments::Create,
            args: {
              issue_id: issue.id,
              description: "reported this issue",
              kind: "update",
              user_id: args.reporter_id,
            },
          )
        else
          errors.merge!(issue.errors)
        end
        issue
      end

      # To help Rails form builders when using this interaction as a form object.
      def to_model
        Issue.new
      end

    end
  end
end
```

Differences between this approach and regular ActiveInteractions:

* Inputs are validated via `class ArgsContract` rather than input filters.
* When refering to inputs, use `args`/`args.l_var` instead of `inputs`/local variables.

# Conventions for interactions

* We pass it the same arguments we would pass to a Rails controller (Hash with values of basic Ruby types only). We do this so that the interaction can be invoked easily, no matter the context since all inputs are of basic types. That means we pass `user_id` instead of a `User` record.
* Naming:
    * When an interaction operates on an ActiveRecord class, we use the following naming:
        * Workspace: The workspace the interaction belongs to. In the example this is `Comms` for "communications".
        * ActiveRecord class pluralized. In the example we use `Issues`. We pluralize to avoid naming conflicts with the model class.
        * Action verb: A verb that describes what this interaction does. In the example `Create` an issue.
    * When an interaction wraps an external service, we use the following naming:
        * Workspace: For example `Authentication`
        * The external service, e.g., `Sso`
        * Action verb: For example `Authenticate`

# Dependency injection

To facilitate testing we use dependency injection via `dry-container`. This allows us to stub/mock dependencies that are unsuitable for testing:

## Usage

Set up an app-wide dependencies container and register all dependencies you want to inject:

```ruby
# config/initializers/deps_container.rb

# Registers services in app wide container for dependency injection
class DepsContainer
  extend Dry::Container::Mixin
end

# Git gem to interact with git repositories
DepsContainer.register(:git, Git)
```

Resolve the dependencies as needed for regular use:

```ruby
# apps/interactions/nw_app_structure/nw_patches/apply.rb
  ...

  def commit_changes_to_git
    git_repo = DepsContainer[:git].open(Rails.root)
    git_repo.add(@file_to_commit)
    git_repo.commit("<the git commit message>")
  end

  ...
```

When testing this interaction, you can stub/mock the `git` dependency to avoid creation of unwanted git commits in the app's git repo:

```ruby
# test/interactions/nw_app_structure/nw_patches/apply_test.rb
require 'dry/container/stub'

# before stub you need to enable stubs for specific container
DepsContainer.enable_stubs!
# Replace the `Git` class with your own mock class for testing:
DepsContainer.stub(:git, MockGit)

```

More info on [stubbing/mocking with Minitest](https://semaphoreci.com/community/tutorials/mocking-in-ruby-with-minitest).

# How this works

* ActiveInteraction provides very capable interactions, however the built in input validation is somewhat lacking.
* dry-validation provides very powerful tools to validate interaction inputs.

To combine the two, we use this snippet of code:

```ruby
module ActiveInteractionWithDryValidation

  extend ActiveSupport::Concern

  # Represents the inputs provided to the interaction after they were validated by dry-validation.
  class Args

    # @param args [Hash] (@see ArgsContract in calling Interaction)
    # @param args_contract_class [Class] the contract class
    def initialize(args, args_contract_class)
      @args = args
      @args_contract_class = args_contract_class
      # Create attr_accessor for each top level schema key and assign the value
      @args_contract_class::SCHEMA.key_map.each { |key|
        self.class.class_eval { attr_accessor key.name }
        send("#{key.name}=", @args[key.name.to_sym])
      }
    end

    def to_h
      @args
    end

    def validate
      @args_contract_class.new.call(@args)
    end

  end

  included do
    # Use ActiveInteraction type casting to convert `inputs` into `args` (object of class Args).
    object :args, class: Args, converter: ->(args) { Args.new(args, self::ArgsContract) }
    # Validate args using dry-validation
    validate :validate_args
  end

  def validate_args
    r = args.validate
    return true if r.success?

    # Convert dry-validation errors to ActiveInteraction/ActiveModel ones:
    # * Convert `nil` keys to `:base`. dry-validation uses `nil`, whereas ActiveInteraction expects `:base`
    # * Flatten nested error keys:
    #   { issue_args: { title: ["is missing"]}} => { issue_args_title: ["is missing"] }
    # * Add an error for each message under a given key.
    compatible_errors = {}
    r.errors.to_h.each { |k, messages_or_hash|
      effective_key = k || :base
      extract_nested_messages(compatible_errors, [effective_key], messages_or_hash)
    }
    compatible_errors.each { |error_key, messages|
      messages.each { |message| errors.add(error_key, message) }
    }
  end

  # @param messages_collector [Hash] will be mutated in place with new messages as we find them
  # @param key_stack [Array<String, Symbol>] the stack of keys we are processing
  # @param value [Array, Hash] if it's an Array, extract messages. If it's a hash, process recursively
  def extract_nested_messages(messages_collector, key_stack, value)
    case value
    when Hash
      # Recursively process
      value.each { |k, v| extract_nested_messages(messages_collector, key_stack << k, v) }
    when Array
      # Add value as message to current key_stack
      effective_key = key_stack.map(&:to_s).join("_").to_sym
      messages_collector[effective_key] ||= []
      messages_collector[effective_key].concat(value)
    else
      raise "Handle this: #{key_stack.inspect}, #{value.inspect}"
    end
  end

end
```
