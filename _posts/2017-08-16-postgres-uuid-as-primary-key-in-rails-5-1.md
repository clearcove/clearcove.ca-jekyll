---
title: PostgreSQL UUID as primary key in Rails 5.1
layout: post
permalink: /2017/08/postgres-uuid-as-primary-key-in-rails-5-1
---

You may consider using PostgreSQL UUIDs as primary keys in your Rails 5.1 app. This article looks at the tradeoffs and provides instructions for how to use them with PostgreSQL and Rails 5.1. Since this decision is hard to change once you get going, I spent a good chunk of time reviewing the pros and cons of UUIDs.

## Benefits of UUID primary keys

* You get the ability to assign primary key values in distributed systems without worrying about collisions:
    * You can create new records in a browser based client app without doing a database roundtrip to get the new record's primary key. This is especially useful where you may at the same time create child records that need to reference the parent record using a foreign key.
    * You can shard your database without getting primary key collisions (at least at an extremely low probability).
* Record ids are not enumerable. This eliminates a threat vector where an attacker could guess URLs based on integer sequences to access resources on your API or web app.
* Non-sequential record ids prevent a 3rd party from getting insights into the size and growth of your data, e.g., how many user records there are in your database. Note: You could accomplish the same outcome by using integer based primary keys internally and use (non-primary-key) UUIDs for public URLs.
* UUIDs prevent silly typo errors like deleting the wrong record because you typed `1123` instead of `123`. If you get one character in a UUID wrong, chances are that it will not be a valid key and nothing will happen.

## Drawbacks of UUID primary keys

* Your data will require more space both in memory and on disk: UUID keys (16 bytes) take up 2 times as much space as `bigint` (8 bytes) and 4 times as much as `int` (4 bytes) primary keys. This won't matter as long as your indexes fit entirely inside of RAM. However if your resources are limited, and this is enough to push you over the limit, then the performance impact will be significant. Remember that UUIDs are stored not only in primary key columns, but in all foreign key columns as well.
* UUIDs are a pain to deal with if you have to remember, type, or communicate them.
* Rows will be sorted randomly by default and `ActiveRecord's` `User.first` and `.last` will not work as expected without an explicit `order` clause.

## Others' opinions

* [Why Auto Increment Is A Terrible Idea](https://www.clever-cloud.com/blog/engineering/2015/05/20/why-auto-increment-is-a-terrible-idea/) argues strongly for UUIDs.
* [Performance considerations for different UUID approaches?](https://www.postgresql.org/message-id/20151222124018.bee10b60b3d9b58d7b3a1839%40potentialtech.com) speaks to performance issues.
* [PostgreSQL primary key type analysis](http://gosimple.me/postgresql-primary-key-type-analysis/) contains in-depth benchmarks for different primary key types.

## How to set up UUID primary keys in Rails 5.1

After weighing the pros and cons, I decided to go with UUIDs for one of my Rails projects. Below are the steps to setting up Rails 5.1 to use UUIDs as primary keys:

### Enable pgcrypto extension

Run this migration:

```bash
$ bin/rails g migration enable_pgcrypto_extension
```

```ruby
class EnablePgcryptoExtension < ActiveRecord::Migration[5.1]
  def change
    enable_extension 'pgcrypto' unless extension_enabled?('pgcrypto')
  end
end
```

Note that Rails 5.1 switched from using the `uuid-ossp` extension's `uuid_generate_v4()` function to using the `pgcrypto` extension and its `gen_random_uuid()` function to generate UUIDs.

### Change the default column type for primary keys

Configure your migration generator to set `id: :uuid` for new tables:

```ruby
# config/initializers/generators.rb

Rails.application.config.generators do |g|
  g.orm :active_record, primary_key_type: :uuid
end
```

Now any new tables will use `:uuid` as type for the primary key column.

### How to handle foreign keys

By default Rails uses `Integer` columns for foreign keys. You need to specify the `:uuid` type for foreign keys:

```ruby
# when creating a new table
t.references :user, type: :uuid
```

or 

```ruby
# when modifying an existing table
add_column :books, :user_id, :uuid
```

## More info

* Wikipedia article about [UUID](https://en.wikipedia.org/wiki/Universally_unique_identifier)
* Rails guide on [PostgreSQL extensions](http://edgeguides.rubyonrails.org/active_record_postgresql.html)
