# Simple Sinatra MVC Template

## What's included

* [Slim](http://slim-lang.com/)
* SASS support via NPM
* PostgreSQL gem (pg)
* Pony
* Rubocop
* [Webpack](https://webpack.js.org/)
* [Tailwind CSS](https://tailwindcss.com/docs/configuration)
* [TypeScript](https://www.typescriptlang.org/)
* A lot more

## Table of contents

* [Getting Started](#getting-started)
* [Installation](#installation)
* [Testing](#testing)
* [Configuration](#configuration)
* [Rake Tasks](#rake-tasks)
* [Asset Pipeline](#asset-pipeline)
* [Pre-deployment](#pre-deployment)
* [Deployment Guides](#deployment-guides)


## Getting Started

``` bash
$ git clone --depth 1 git://github.com/kathgironpe/simple-sinatra-mvc.git myapp
$ rm -r myapp/.git && rm myapp/README.md
```

## Installation

### Use bundler to install gems

``` bash
$ bundle install --no-deployment
```

### Use npm to install some dependencies

```bash
$ npm i
```

### Start the server

``` bash
$ rackup
```

## Testing

### Unit tests

Use **RSpec** for unit tests and functional tests.

``` bash
$ bundle exec rake spec
```

### Acceptance tests

For acceptance tests, some example is also provided. Use:

```bash
cucumber
```

Or use the rake task **lib/tasks/test.rake**:

```
rake features
```

## Configuration

``` bash
$ cp config/database.yml.example config/database.yml
```

Update `database.yml`.


By default, we use PostgreSQL.

To install PostgreSQL on a Mac, you might need homebrew.

```bash
$ brew install postgresql
```

Creating a database should be as simple as:

```bash
$ createdb database_name
```

Normally, you already have a **postgres** user with a blank password. Otherwise, please refer to the PostgreSQL documentation and create a new user.

You may have to update **config.ru** and files on **config** directory as needed.

## Rake Tasks

``` bash
$ rake -T
```

This returns this output:

```bash
rake db:create              # Creates the database from DATABASE_URL or config/database.yml for the current RAILS_ENV (use db:create...
rake db:create_migration    # Create a migration (parameters: NAME, VERSION)
rake db:drop                # Drops the database from DATABASE_URL or config/database.yml for the current RAILS_ENV (use db:drop:all...
rake db:environment:set     # Set the environment value for the database
rake db:fixtures:load       # Loads fixtures into the current environment's database
rake db:migrate             # Migrate the database (options: VERSION=x, VERBOSE=false, SCOPE=blog)
rake db:migrate:down        # Runs the "down" for a given migration VERSION
rake db:migrate:redo        # Rolls back the database one migration and re-migrates up (options: STEP=x, VERSION=x)
rake db:migrate:status      # Display status of migrations
rake db:migrate:up          # Runs the "up" for a given migration VERSION
rake db:prepare             # Runs setup if database does not exist, or runs migrations if it does
rake db:reset               # Drops and recreates the database from db/schema.rb for the current environment and loads the seeds
rake db:rollback            # Rolls the schema back to the previous version (specify steps w/ STEP=n)
rake db:schema:cache:clear  # Clears a db/schema_cache.yml file
rake db:schema:cache:dump   # Creates a db/schema_cache.yml file
rake db:schema:dump         # Creates a database schema file (either db/schema.rb or db/structure.sql, depending on `config.active_r...
rake db:schema:load         # Loads a database schema file (either db/schema.rb or db/structure.sql, depending on `config.active_rec...
rake db:seed                # Loads the seed data from db/seeds.rb
rake db:seed:replant        # Truncates tables of each database for current environment and loads the seeds
rake db:setup               # Creates the database, loads the schema, and initializes with the seed data (use db:reset to also drop ...
rake db:structure:dump      # Dumps the database structure to db/structure.sql
rake db:structure:load      # Recreates the databases from the structure.sql file
rake db:version             # Retrieves the current schema version number
rake features               # Run Cucumber features
rake spec                   # Run RSpec code examples
```

To create a database for a specific environment, do:

``` bash
$ rake db:create RACK_ENV=production
```

The default environment is "development"

To create a migration file called "create_pages", do:

``` bash
$ rake db:create_migration NAME=create_pages
```

To do migration:

``` bash
$ rake db:migrate RACK_ENV=production
```

To rollback:

``` bash
$ rake db:rollback RACK_ENV=production
```

The default is development so this should just work:

``` bash
$ rake db:migrate
```

To start the server, use **rackup**:

```bash
$ rackup
```

## Asset Pipeline

As a 7-year-old project as of April 2021, I have changed my mind many times about asset management.
From Sprockets to Brunch.js and now Webpack.js, the changes are largely based on experience and community support.
Using Webpack makes sense for asset management. Currently, even Ruby on Rails uses this.

By default, we have the following supported directories:

* app/assets/javascripts
* app/assets/stylesheets
* app/assets/files

Your non-JavaScript and non-CSS files should go to `app/assets/files` directory.


### Why Webpack?

Used and trusted by a lot. The documentation is very clear.

### Configuring Webpack

Please take some time to read the [documentation](https://webpack.js.org/) before updating **webpack.config.js**.

## Pre-deployment

### Review Gitignore file

You likely have to remove some entries on the **.gitignore** file like **config/database.yml** if you are deploying on Heroku or OpenShift.

## Deployment Guides

Deployment to Heroku and OpenShift should fairly be easy. We rely on **postinstall** on **package.json** to build the assets. Regardless of where you deploy your app, you need the following installed:

1. Node.js
2. Latest stable Ruby version

The command **npm install** or **npm i** will install Node.js packages and build the assets using **brunch**.

### Heroku

Deploying a Sinatra or Ruby on Rails application on Heroku requires some **buildpacks**.

```
heroku buildpacks                     # view current buildpacks
heroku buildpacks:clear               # clear current buildpacks, if necessary
heroku buildpacks:add heroku/nodejs   # add the Node.js buildpack
heroku buildpacks:add heroku/ruby     # add the Ruby buildpack
```

You generally don't have to worry about building the assets as long you have the same `package.json` on this repository.
Please read **build**, **postinstall**, and **deploy** scripts.

Other necessary steps: please read **pre-deployment**.
