---
layout: post
title:      "Rails Generators"
date:       2020-03-26 17:03:51 +0000
permalink:  rails_generators
---


Amongst the things I found myself _constantly_ referencing while building my first Rails app were **Rails generators**. You can check out the [Rails Guides][1] for more official documentation.

[Rails Guides - Generate][2]

## What They Do

---

Rails generators save a lot of time when building out your project. The `rails g` CLI command, AKA `rails generate` uses templates to build out different portions of your Rails application. Certain gems will also come with their own generators, and you can install some on your own to tailor your personal project. If you want to see what types of generators are available to you, run the following command from the terminal:

```terminal
$ rails generate
```

When I run `rails generate` from the root of my Rails project, I get the following output to my terminal.

_Your output may vary as I have installed gems that include their own generators_

```terminal
General options:
  -h, [--help]     # Print generator's options and usage
  -p, [--pretend]  # Run but do not make any changes
  -f, [--force]    # Overwrite files that already exist
  -s, [--skip]     # Skip files that already exist
  -q, [--quiet]    # Suppress status output

Please choose a generator below.

Rails:
  application_record
  assets
  channel
  controller
  generator
  helper
  integration_test
  jbuilder
  job
  mailbox
  mailer
  migration
  model
  resource
  responders_controller
  scaffold
  scaffold_controller
  system_test
  task

ActiveRecord:
  active_record:application_record
  active_record:devise

Devise:
  devise
  devise:controllers
  devise:install
  devise:views
  devise:views:bootstrap_templates
  devise:views:locale

FactoryGirl:
  factory_girl:model

Mongoid:
  mongoid:devise

Responders:
  responders:install

Rspec:
  rspec:controller
  rspec:feature
  rspec:generators
  rspec:helper
  rspec:install
  rspec:integration
  rspec:job
  rspec:mailer
  rspec:model
  rspec:observer
  rspec:request
  rspec:scaffold
  rspec:system
  rspec:view

TestUnit:
  test_unit:channel
  test_unit:generator
  test_unit:mailbox
  test_unit:plugin
```

The basic ones that I will focus on in this post are for **Rails** and the **[Devise][6] gem**

### `rails g migration`

---

To get some information about `rails g migration` you can type the following in your terminal:

```terminal
$ rails generate migration --help
```

We can learn what it does by checking out the description that outputs to the terminal:

```terminal
Description:
    Stubs out a new database migration. Pass the migration name, either
    CamelCased or under_scored, and an optional list of attribute pairs as arguments.

    A migration class is generated in db/migrate prefixed by a timestamp of the current date and time.

    You can name your migration in either of these formats to generate add/remove
    column lines from supplied attributes: AddColumnsToTable or RemoveColumnsFromTable
```

To create a projects table we could enter the following:

```terminal
$ rails g migration CreateProjects
```

Rails would **create** a migration file for us in db/migrate: 20200326032030_create_projects.rb

```ruby
class CreateProjects < ActiveRecord::Migration[5.0]
  def change
  end
end
```

We could add attributes and have Rails figure it out for us, but I will show you why I'm not later in this post.

[Rails Guides - Creating Migrations][3]

### `rails g model`

---

To get some information about `rails g model` you can type the following in your terminal:

```terminal
$ rails generate model --help
```

We can learn what it does by checking out the description that outputs to the terminal:

```terminal
Description:
    Stubs out a new model. Pass the model name, either CamelCased or
    under_scored, and an optional list of attribute pairs as arguments.

    Attribute pairs are field:type arguments specifying the
    model's attributes. Timestamps are added by default, so you don't have to
    specify them by hand as 'created_at:datetime updated_at:datetime'.

    As a special case, specifying 'password:digest' will generate a
    password_digest field of string type, and configure your generated model and
    tests for use with Active Model has_secure_password (assuming the default ORM
    and test framework are being used).

    You don't have to think up every attribute up front, but it helps to
    sketch out a few so you can start working with the model immediately.

    This generator invokes your configured ORM and test framework, which
    defaults to Active Record and TestUnit.

    Finally, if --parent option is given, it's used as superclass of the
    created model. This allows you create Single Table Inheritance models.

    If you pass a namespaced model name (e.g. admin/account or Admin::Account)
    then the generator will create a module with a table_name_prefix method
    to prefix the model's table name with the module name (e.g. admin_accounts)
```

If you entered the following in your terminal:

```terminal
$ rails g model Project
```

Rails would create a migration file similar to this:

```ruby
class CreateProjs < ActiveRecord::Migration[6.0]
  def change
    create_table :projs do |t|

      t.timestamps
    end
  end
end
```

It would also create our model in app/models/project.rb:

```ruby
class Project < ApplicationRecord
end
```

Once again, we could add attributes and have Rails figure it out for us, but I will show you why I'm not in this next section.

[Rails Guides - Model Generators][4]

### `rails g resource`

---

To get some information about `rails g resource` you can type the following in your terminal:

```terminal
$ rails generate resource --help
```

We can learn what it does by checking out the description that outputs to the terminal:

```terminal
Description:
    Stubs out a new resource including an empty model and controller suitable
    for a RESTful, resource-oriented application. Pass the singular model name,
    either CamelCased or under_scored, as the first argument, and an optional
    list of attribute pairs.

    Attribute pairs are field:type arguments specifying the
    model's attributes. Timestamps are added by default, so you don't have to
    specify them by hand as 'created_at:datetime updated_at:datetime'.

    You don't have to think up every attribute up front, but it helps to
    sketch out a few so you can start working with the model immediately.

    This generator invokes your configured ORM and test framework, besides
    creating helpers and add routes to config/routes.rb.

    Unlike the scaffold generator, the resource generator does not create
    views or add any methods to the generated controller.
```

Basically, each of the generators so far has built on the previous.

#### Rails will create four main things with the `resource` generator

1. A model

2. A migration

3. An empty controller

4. `resources :projects` in `routes.rb` (`:projects` because that is the resource I will be generating)

The `resource` generator is one I like because I prefer to build out my own views. If you entered the following in your terminal:

```terminal
$ rails g resource Project --no-test-framework
```

You would end up seeing something like this:

```terminal
Running via Spring preloader in process 53386
      invoke  active_record
      create    db/migrate/20200326023754_create_projects.rb
      create    app/models/project.rb
      invoke  controller
      create    app/controllers/projects_controller.rb
      invoke    erb
      create      app/views/projects
      invoke    helper
      create      app/helpers/projects_helper.rb
      invoke    assets
      invoke      scss
      create        app/assets/stylesheets/projects.scss
      invoke  resource_route
       route    resources :projects
```

#### Migration and Model

The first part will create our migration and our model files `20200326023754_create_projects.rb` && `proj.rb` I added `--no-test-framework` in the terminal because I did not plan on writing tests. As I have written it, it will only create empty migrations. To have rails generate the migrations for us, we can do something like this instead:

```terminal
$ rails g resource Project title:string description:string developers completed:boolean user:reference --no-test-framework
```

When we ran `rails generate resource --help` earlier, we saw this at the bottom:

```terminal
Examples:
    `rails generate resource post` # no attributes
    `rails generate resource post title:string body:text published:boolean`
    `rails generate resource purchase order_id:integer amount:decimal`
```

Since on the second example, we had put in attributes, we can open our migration and see that [Rails][1] magic has created our migration for us!

```ruby
class CreateProjects < ActiveRecord::Migration[6.0]
  def change
    create_table :projects do |t|
      t.string :title
      t.string :description
      t.integer :developers
      t.boolean :completed
      t.references :user, null: false, foreign_key: true

      t.timestamps
    end
  end
end
```

The projects table now has fields for a **title** and **description** which are _strings_, the number of **developers** as an _integer_, a _boolean_ value for whether or not it has been completed and **_references_** to a **User** with a foreign key.

#### Controller

`rails g resource` leaves us with an empty conroller where we can define our methods for our routes. If we had decided to do `rails g scaffold` we would have had seven public methods (`index`, `show`, `new`, `edit`, `create`, `update`, `destroy`) and two private methods (`set_project` and `project_params`). Although this can be a good way to get up and running quick, but you will often be left with a lot of **bloat** in your code.

#### Routes

By default we will be left with:

```ruby
resources :projects
```

in config/routes.rb, but we can modify it later as we create our views and controller actions.

#### Helpers

We will also be left with a **helper** file in: `app/helpers/projects_helper.rb` and an **SCSS** file for styling in `app/assets/stylesheets/projects.scss`

[Rails Generators - Resource][5]

## [Devise][6] Generators

The last thing that I wanted to cover was generators for the **[Devise][6]** gem. I found it useful to generate the views for the **[Devise][6]** controllers that are created by the gem. This can prove useful if you want to change how the sign-in, login/logut, edit user pages appear in the browser. I decided to do so because I felt that the automatic views were a little lackluster and wanted more configuration.

### Installing the [Devise][6] gem

Installation is pretty simple. We need to make sure to add:

```ruby
gem "devise"
gem "omniauth"
gem "omniauth-google-auth2"
```

to our `Gemfile` as well as any other gems we may want to use with **[Devise][6]** such as **[Omniauth][7]** and **[Google OAuth2][8]**. Then we can enter into our terminal:

```terminal
$ rails generate devise:install
```

### Generate [Devise][6] Models

Generating models for **[Devise][6]** is pretty simple. It is just like we would do in **[Rails][4]**, with the addition of `devise` before the model name:

```terminal
$ rails generate devise User
```

We can do some modification but **[Devise][6]** will give use something like this:

```ruby
class DeviseCreateUsers < ActiveRecord::Migration[5.1]
  def change
    create_table :users do |t|
      ## Database authenticatable
      t.string :email,              null: false, default: ""
      t.string :encrypted_password, null: false, default: ""

      ## Recoverable
      t.string   :reset_password_token
      t.datetime :reset_password_sent_at

      ## Rememberable
      t.datetime :remember_created_at

      ## Trackable
      t.integer  :sign_in_count, default: 0, null: false
      t.datetime :current_sign_in_at
      t.datetime :last_sign_in_at
      t.string   :current_sign_in_ip
      t.string   :last_sign_in_ip

      ## Confirmable
      # t.string   :confirmation_token
      # t.datetime :confirmed_at
      # t.datetime :confirmation_sent_at
      # t.string   :unconfirmed_email # Only if using reconfirmable

      ## Lockable
      # t.integer  :failed_attempts, default: 0, null: false # Only if lock strategy is :failed_attempts
      # t.string   :unlock_token # Only if unlock strategy is :email or :both
      # t.datetime :locked_at


      t.timestamps null: false
    end

    add_index :users, :email,                unique: true
    add_index :users, :reset_password_token, unique: true
    # add_index :users, :confirmation_token,   unique: true
    # add_index :users, :unlock_token,         unique: true
  end
end
```

Where we can comment and uncomment stuff that we will use in our project. It will also create a user model in app/models/user.rb which will have stuff we can comment/uncomment:

```ruby
class User < ApplicationRecord
  # Include default devise modules. Others available are:
  # :confirmable, :lockable, :timeoutable and :omniauthable
  devise :database_authenticatable, :registerable,
         :recoverable, :rememberable, :trackable, :validatable
end
```

depending on what features our application requires.

[Rails Girls Guides - Devise][9]

As you can see Rails really makes building an application **magical**!

> **“Rails is the killer app for Ruby.”**
>
> **_Yukihiro Matsumoto_**, _Creator of Ruby_

---

#### _**Disclaimer**_

_I am a student at Flatiron School, studying web development. If you see any errors in this post, please let me know. I am most quickly found on Twitter @rukshanuddin_

[1]: https://guides.rubyonrails.org
[2]: https://guides.rubyonrails.org/command_line.html#rails-generate
[3]: https://guides.rubyonrails.org/active_record_migrations.html#creating-a-migration
[4]: https://guides.rubyonrails.org/active_record_migrations.html#model-generators
[5]: https://medium.com/@josephdlawson21/using-the-generate-command-in-rails-9c738380f2d9#5eb8
[6]: https://github.com/heartcombo/devise
[7]: https://github.com/omniauth/omniauth
[8]: https://github.com/zquestz/omniauth-google-oauth2
[9]: https://guides.railsgirls.com/devise

