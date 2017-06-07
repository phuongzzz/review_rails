"# Review Rails tutorial

## Table of contents

- [Chapter 3](#chapter-3)
- [Chapter 4](#chapter-4)
- [Chapter 5](#chapter-5)
- [Chapter 6](#chapter-6)
- [Chapter 7](#chapter-7)
- [Chapter 8](#chapter-8)
- [Chapter 9](#chapter-9)
- [Chapter 10](#chapter-10)
- [Chapter 11](#chapter-11)
- [Chapter 13](#chapter-13)
- [Chapter 14](#chapter-14)

## Chapter 3

Quick summary:

- Generators in Rails:
  - `controller` generate
    - controller file
    - action view template
    - controller test file
    - controller helper
    - assets (js, css)
  - `model`
    - migration file
    - model file
    - model test file
    - fixtures for model
  - `integration_test`
    - integration test file
  - `scaffold`
    - MVC assets
    - resource routes
    - migration file
  - `mailer`
    - mailer file
    - mailer erb template file
    - mailer preview file
    - mailer test file
  - `migration`
    - migration file
- Generator options:
  - `-h`: help
  - `-s`: skip existing file(s)
  - `-f`: force overwrite existing file(s)

---

## Chapter 4

Quick summary:

- Rails helper
  - any rails helper are available to any view
- String and methods
  - String, comments, object, functions
- Data structure:
  - Array
    - `split`
    - `first`
    - `second`
    - `last`
    - `empty?`
    - `include`
    - `join`
    - `push`
  - Block
    - `.each...do`
  - Hash
    - `each...do`


---

## Chapter 5

Quick summary

- `Bootstrap`
  - install gem
- `partial`
  - create and include to `application` layout
- `sass`
  - all css assets will be included in `application.css` file
- Rails routes
  - use `_path`: all, except `redirect`
  - use `_url`: just for `redirect`
- named routes
  - `_path`
- integration test

---

## Chapter 6

Quick summary

- migration files:
  - describe changes for db
- `schema.rb`: keep current state of db

- Active Record methods
  - `new` (create new AR object, in memory)
    - return `nil` if no arg
    - return obj if created
  - `save` (save to db)
    - return `true`/`false`
  - `valid?`: check object validation
  - `new` + `save` = `create`
    - return object
  - `destroy`
    - return obj
  - `find(id)`: find object by id
  - `find_by(attribute: "[value]")`: find object by some attribute
  - `first`: get the first user
  - `all`: get all user
  - `find_by_attribute`: find by some attribute
  - `object.attribute`: update object's attribute
  - `reload`: reload object
  - `update_attributes`:
    - validate
    - return `boolean`
    - update many attributes
  - `update_attribute`:
    - no validation
    - update single validate
- Model validation:
  - `presence: boolean`
    - validate the existence of model's attributes
  - `length`
    - validate the `maximum` and the `minimum` length of model's attributes
  - `format`
    - validate the formation of object's attribute, using regular expression
  - `uniqueness`
    - validate the uniqueness of model's attributes
      - options:
        - `scope`: specify one more attributes to limit uniqueness check
        - `case_sensitive`: distinguish between uppercase or not
    - in model level
    - in db level
      - using `before_save callback`
- callback:
  - functions that automatically called at a particular point in the lifecycle of Active Record object
  - some regular used callback in Rails
    - `before_save`: will be invoked automatically before Active Record object trying to save to db
    - `before_validation`: will be called before object passed through the validation
    - `before_create`: will be called before object is saving to the db
    - `before_action`: will be called before particular action

- Add password
  - `has_secure_password`
    - require `password_digest` attribute on model
    - must have `presence` validation for password
    - generate 2 virtual attributes: `password` and `password_confirmation`
    - `authenticate` method
      - return `false` when fail
      - return `object` if success

---

## Chapter 7

Quick summary:

- `debug` method
- `params` hash
- Rails env
  - `development`
  - `test`
  - `production`
- REST in Rails
  - represent data as *resource*
  - can CRUD via HTTP method
  - resource can be referenced by `/resource_name/:id`
- `debugger`
- `render`
  - get the corresponding template, not count as request
- `redirect`
  - get the corresponding template, count as HTTP request
- `form_for`
  - take an `Active Record` object
  - build form using object's attributes
  - called method correspoding to HTML form element -> return code for that element for creating object's attributes
  - create a `hash` via `params` variable for creating user using form's value
  - Differences between `form_for` and `form_tag`
    - `form_for` is used when creating specific model or resource
      - => used with Active Record object (model)
      - no need to specify `url` and `method`
    - `form_tag` is used when creating basic form
      - used for non-model form (like search form)
      - need to specify `url` and `method`
- Strong params (security purpose)
  - `require` which attributes needed
  - `permit` attributes
- `pluralize` method
- `redirect_to` and `flash`

---

## Chapter 8

- Quick summary
  - `session`: semi-permanent connection between two computers
  - `cookies`: small text placed on user's browser
  - `form_for` for not an `Active Record` object
    - include name of resource
    - include corresponding URL
  - `session` method: place temporary `cookies` on user's browser (lost when browser close)
    - set value: `session[:key] = value`
    - get value: `session[:key]`
    - delete: `session.delete(:key)`

----

## Chapter 9

Quick summary:

- virtual attributes: attributes in *model* level, not has corresponding *database* column
  - create one: `attr_accessor :attribute_name`
- `self`
  - access for current model's attributes
- `cookies` method
  - set value:
    - `cookies[:key] = value`
    - `cookies.permanent[:key] = value`
    - *more secure*: `cookies.signed[:key] = value`
  - get value:
    - `cookies[:key]`
  - delete:
    - `cookies.delete(:key)`

---

## Chapter 10

Quick summary:

- send `PATCH` request by browser
  - insert *hidden* input with `value="patch"` attribute
- distinguish *update* and *create*
  - `object.new_record?`
    - `true`: `POST` => create
    - `false`: `PATCH` => update
- `before_action` callback
  - specify which function should be run before some actions
- foward user:
  - get current url: `request.original_url`
  - get current request method: `request.get?`
- faking sample data:
  - `faker` gem
- pagination:
  - `will_paginate` and `bootstrap-will_paginate` gem

---

## Chapter 11

Quick summary

- only use specifiec named routes for action in REST :
  - `resources :resource_name, only: [:action]`
- `before_create` callback
  - call before object is created in memory


- Mailer (review later)
  - generate mailer: `rails generate mailer UserMailer account_activation password_reset`
    - `UserMailer`: name of the mailer
    - `account_activation` and `password_reset`: mailer's method
    - this will generate:
      - 2 views:
        - text mailer template
        - HTML mailer template
- **metaprograming???**

---

## Chapter 12

---

## Chapter 13

Techiques:

- `references` type in `Active Record` data type
- `belongs_to` and `has_many` relationships
- association between models:
  - example: `micropost belongs_to user` and `user has_many microposts`
    - create new micropost: `user.micropost.create` or `user.micropost.build` or `user.micropost.create!`
- default scope:
  - option in SQL's statements
- dependent:
  - make some model dependent on some actions
- get model(s) from db
  - `model_name.where([sql_statement])`
- upload image:
  - `carrierwave, mini_magick, fog` gem
  - make uploader using CarrierWave (CW) functionality: `rails generate uploader Picture`
  - image upload with CW can associate with model by
    - `picture:string` field in Active Record model
    - `mount_uploader :picture, PictureUploader`
  - upload form:
    - `file_field :picture`
    - *picture* is an attribute above
- jQuery/Javascript

---

## Chapter 14

Quick summary

- nested resource
  - config in `routes.rb`


- `ajax`:
  - allow web page send requests to server without leaving page
  - how to use:
    - add `remote: true` as arg for `form_for`
  - respond to `ajax` request
    - `respond_to` method
- brief SQL statement


