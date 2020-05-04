---
title: ActiveInteraction with dry-validation
layout: post
permalink: /2020/05/active-interaction-with-dry-validation
---

The [ActiveInteraction](https://github.com/AaronLasseigne/active_interaction) gem is super useful, however we like to use [dry-validation](https://github.com/dry-rb/dry-validation) for input argument validation.

The example code illustrates how the two can be combined. The example uses an interaction to close an issue. This is what the interaction looks like:

``` ruby
module Issues
  # Closes an issue. Use like so:
  #     Issues::Close.run(args: { id: 42, resolution: "fixed", user_id: 17 })
  class Close < ApplicationInteraction

    class ArgsContract < Dry::Validation::Contract

      SCHEMA = Dry::Schema.Params do
        optional(:comment).hash do
          required(:description).filled(:str?)
          required(:kind).value(included_in?: ["comment", "update"])
        end
        required(:id).filled(:int?)
        required(:resolution).value(included_in?: ["duplicate", "fixed", "wont_fix"])
        required(:user_id).filled(:int?)
      end

      # Optional validation rules
      # rule(:resolution) do
      #   ...
      # end

    end

    # `class:`` evaluates to "Issues::Close::Args", see ApplicationInteraction.
    # `converter: :new` calls `Issues::Close::Args.new` with the inputs as argument.
    object :args, class: self.name + "::Args", converter: :new,
    validate :validate_args # See ApplicationInteraction

    # @return [Issue]
    def execute
      issue = Issue.find(args.id)
      errors.add(:base, "could not find Issue with id #{args.id.inspect}") if issue.nil?

      if args.comment
        compose(
          IssueComments::Create,
          args: {
            issue_id: issue.id,
            description: args.comment[:description],
            kind: args.comment[:kind],
            user_id: args.user_id,
          },
        )
      end

      issue.send(args.resolution_event)
      issue.close!
      issue
    end

  end
end
```

The `ApplicationInteraction` base class provides some convenience methods for a fairly seamless integration:

* Provides a Value object class `Args` that offers convenient dot notation access to each input arg and the integration with dry-validation.
* Declares `validate_args` method that is called via ActiveInteraction's `validate :validate_args` macro.

``` ruby
class ApplicationInteraction < ActiveInteraction::Base

  # A wrapper for the `object :args` input. This value object gets instantiated
  # via the `converter: :new` option on the interaction's input filter.
  class Args

    # Create attr_accessor for each top level schema key
    ArgsContract::SCHEMA.key_map.each { |key| attr_accessor key.name }

    # @param args [Hash] (@see ArgsContract)
    def initialize(args)
      @args = args.stringify_keys # Stringify top level keys for i_var assignment below.
      # Assign each top level schema key to instance variable so that it can be accessed with dot notation
      ArgsContract::SCHEMA.key_map.each { |key| send("#{key.name}=", @args[key.name]) }
    end

    # This method gets called from interaction's #validate_args method
    def validate
      ArgsContract.new.call(@args)
    end

  end

  # This methods gets called from the `validate :validate_args` statement in the interaction.
  def validate_args
    r = args.validate
    errors.merge!(r.errors) unless r.success?
  end

end
```
