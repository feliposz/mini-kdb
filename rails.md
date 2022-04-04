---
title: Ruby on Rails
---

## Concepts

- Opinionated
- DRY: Don't Repeat Yourself
- COC: Convention over Configuration

## Check version, installation

```
ruby -v
rails -v
gem -v
bundle -v
sqlite3 --version
```



## Generator / Scaffolding

Create new app on `./appname`

```
rails new appname --database=mysql
cd ./appname
```

Generate a simple controller `welcome` with a single method `index`:

```
rails generate controller welcome index
```

Generate a scaffold for a model, a controller with standard methods and standard views:

```
rails generate scaffold contacts name:string phone:stringa address:string
```

## Models/database

Enable/start local MySQL server:

```
sudo systemctl enable mysql
sudo systemctl start mysql
```

Generate a model with some fields:

```
rails generate model Item title:string notes:string
```

Create database and generate initial migrations:

```
cd ./appname
rake db:create
rake db:migrate
```

## Routing

Make default (CRUD) routes for a given resource editing `config/routes.rb`

~~~ruby
Rails.application.routes.draw do
  
  # A simple get route on /welcome/index
  get 'welcome/index'

  # Route to render when browsing at root (/)
  root 'welcome#index'
  
  # Make default routes (GET /items, POST /items, etc) to controller
  resources :items
end
~~~


Check routes:

```
rake routes
```

## Tips/Debugging

Render parameters received from post route (for debugging):

~~~ruby
class ItemsController < ApplicationController
    def create
        render plain: params[:item].inspect
    end
end
~~~

## Issues

### SQLite

The `sqlite3 1.4.0` gem seems to be broken.

Try to edit `Gemfile` replacing it with:

```
gem 'sqlite3', '>= 1.3', '< 1.4'
```

Then run `bundle update`.

Or try:

```
gem uninstall sqlite3
gem install sqlite3 --platform=ruby --version 1.3.13
```

### Generators

If you get the error `Could not find generator 'xxx'` when running a generator for a gem you've installed, try stopping `Spring` and running the generator again.

```
spring stop
```
