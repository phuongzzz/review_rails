# Review Rails tutorial

## Table of contents

- [Chapter 3](#chapter-3)
- [Chapter 4](#chapter-4)
- [Chapter 5](#chapter-5)
- [Chapter 6](#chapter-6)

## Chapter 3

Techniques: 

- generating controller 
- undoing things
- understanding basic of generator commands
- TDD
- simple functions: ```setup, provide, yeild```

---

### 3.1 Sample app setup

- create new project: ```rails new sample-app```
- run ```bundle install``` to install gem
  - option: ```--without production``` to skip install ```:production``` group
- make Gemfile and installed gems match: ```bundle update```
- init and commit with git, deploy to heroku

---

### 3.2 Static pages

- create *actions* and *views*

#### 3.2.1 Generated static pages

- generate controller to handle static pages: ```rg controller StaticPages home help```
  - ```StaticPages```: name of controller, name can be ```CamelCase``` or ```snake_case```
  - ```home``` and ```help```: name of controller's acitons
- undoing things:
  - ```rails destroy controller StaticPages home help```
  - ```rd model User```
  - rollback db:
    - ```rails db:rollback```
- generating controller also update ```routes.rb``` file automatically
- ```routes.rb```
  - ```get static_pages/home```
    - map request ```static_pages/home``` to ```home``` action in ```StaticPages``` controller
- ```static_pages_controller.rb```
  - contains ```home``` and ```help``` actions
  - Rails will executes code in these actions, then render the view corresponding to these actions (in this case, view'll located at ```app/views/static_pages/something.html.erb```)

#### 3.2.2 Custom static pages:

- Using html to custom these 2 ```.erb``` files (easy)

---

### 3.3 Getting started with testing

#### 3.3.1 Our first test

- running test: ```rails test``` or ```rt```

#### 3.3.2. Red (making test failed)

- write failed test for ```about``` page (similar to first 2 tests)

#### 3.3.3 Green (make the test pass)

- fix in ```routes.rb```
- fix in ```static_pages_controller.rb```
- add view template (```about.html.erb```)
- ```rt``` --> ```green```

---

### 3.4 Slightly dynamic pages

- change ```app/views/layouts/application.html.erb```'s name

#### 3.4.1 Testing title (making the test fail)

- test title for desired value:
  - ```assert_select "title", "Home | Rails tutorial"```
  - ```rt``` -> failed

#### 3.4.2 Making the test pass

- adding page title
  - go to view template, adding title
  - ```rt``` -> test passed :)
- *exercise*: using ```setup``` function, remove repetition code in test

#### 3.4.3 Layouts and ERB (refactor)

- ```provide``` function
- ```yield``` function
- bring ```application.html.erb``` back
- *exercise*: make ```Contact``` page

#### 3.4.4 Setting root routes

- ```routes.rb```:
  - ```root 'static_pages#home'```
  - *ex*: write test for root path

### 3.5 Conclusion

---

## Chapter 4

Techniques: 

- create helpers
- brief in ruby language

### 4.1 Motivation

#### 4.1.1 Built-in helper

- ```style_sheet_link_tag```: is a custom helper

#### 4.1.2 Custom helpers

- helpers: functions which used in the view
- in ```application_helper.rb``` file, create ```full_title``` helper
- refactor ```application.html.erb``` file

---

## Chapter 5

### 5.1 Adding structure

Techniques:

- ```link_to```
- add ```class``` and ```id``` for erb tags
- ```image_tag```
- split layout to *partials*
- asset pipeline (asset dir, manifest file, preprocessor engine)
- named routes
- integration test
- using helpers in test

#### 5.1.1 Adding navigation

- Add navigation bar in ```application.html.erb``` file
- ```link_to``` helper: ```link_to "google", "www.google.com"```
  - 1st arg: text in the view
  - 2nd arg: URL
- add ```class``` and ```id``` for erb tags: ```<%= link_to "sample app", '#', id: "logo" %>```
- ```yield```: insert content of each page to site layout
- ```image_tag```: ```image_tag("rails.png", alt: "Rails logo")```
  - 1st arg: path to image
    - Rails will look at ```app/assets/images/rails.png```
  - 2nd arg: alt

#### 5.1.2 Bootstrap and custom CSS

- add bootstrap 
  - add gem: ```gem 'boostrap-sass'```, then ```bi```
  - create ```custom.scss```: (in ```app/assets/stylesheets/custom.scss```)
    - add ```@import "bootstrap-sprockets"``` and ```@import "bootstrap"``` to this file
    - write some sass :)
- all css file will be included in application.css

#### 5.1.3 Partials

- include partials: ```<%= render 'layouts/shim' %>```
- create partials:
  - ```app/views/layouts/_shim.html.erb```
- ex: create ```shim, header, footer``` partials

---

### 5.2 Sass and asset pipeline

#### 5.2.1 Asset pipeline

- asset dir
  - ```app/assets```: specific asset for application
  - ```lib/assets```: our library (self written)
  - ```vendor/assets```: lib from outer libraries
- manifest file
  - tell rails how to combine files -> single file (via ```Sprockets``` gem)
- preprocessor engine
  - scss
  - coffee
  - erb

#### 5.2.2 Sass

---

### 5.3 Layout links

- Using named route

#### 5.3.1 Contact page

- *done*

#### 5.3.2 Rails routes

- edit ```routes.rb``` file
- 2 functions: ```root_path``` and ```root_url```
  - ```root_path```: using all scenario, execpt redirect
  - ```root_url```: using in redirect
  - change routes to something like this:
    - ```get  '/help', to: 'static_pages#help'```
    - this will create 2 functions:
      - ```help_path```
      - ```help_url```
  - update test (because this time it'll fail)
  - ```as``` option in routes

#### 5.3.3 Using named routes

- *self solving*

#### 5.3.4 Layout link test

- integration test: end-to-end testing application behavior
- generating test: ```rails generate integration_test site_layout```
  - ```integration_test```: type of test
  - ```site_layout```: test name
- ```assert_template```: verify some template has rendered to the view
- ```asset_select```: ```assert_select "a[href=?]", root_path, count: 2```
  - select tag, value for ```href```, counting
- using helper in test to check title

---

### 5.4 User signup: first step

#### 5.4.1 Users controller

- ```rails generate controller Users new```
- create named route for ```sign_up``` path
- test it! (for template, title,...)

---

### 5.5 Conclusion

---

## Chapter 6

Techniques:

- Active Record
- generating model
- migration (create/rollback)
- CRUD model in console
- model validations: presence, format, length, uniqueness (model and db level)
  - callback
  - regex
- TDD flow:
  - write test first
  - test fail -> fix
  - test passed :)

### 6.1 User model

- first step: making data structure to capture and store users's information
- Active Record: library for interacting with db

#### 6.1.1 DB migrations

- generating model: ```rg model User name:string email:string```
  - ```User```: name of the model (singular)
  - ```name:string```: attributes and type
  - after this command, *migration* file is generated
  - run migration: ```rails db:migrate```
  - ```schema.rb``` keep track of current state of db

#### 6.1.2 The model file

- ```app/models/user.rb```

#### 6.1.3 Creating user object

- ```rcs```: ```rails console --sandbox```: rollback changes after exit
- ```User.new```: make new user in *memory* (not db) return obj
- to save user -> db: ```user.save``` (return ```true/false```)
- make + save model at the same time: ```user.create``` (return object)
- ```user.destroy```: delete model

#### 6.1.4 Finding user obj

- find user with id: ```User.find(1)```
- find by specific attributes: ```User.find_by(email: "phuong@gmail.com")```
  - issue: poor performance in large system
- get first user: ```User.first```
- get all users: ```User.all```
- *another way:* ```User.find_by_name``` (is it work with ```email```???)
  - yes, it ***works!!!***

#### 6.1.5 Updating user objects

There are 3 ways to update attributes:

- update individually:
  - ```user.name = phuongnew@gmail.com```
  - ```user.save```
  - *if not save, update will lost when call ```user.reload```*
- ```update_attributes``` method:
  - ```user.update_attributes(name: "Phuong Phuong", email: "phuongphuong@gmail.com")```
  - this method will run ```update``` and ```save``` on success
  - return ```true```
  - if validation fail, this method will fail ***this method check the validation***
- ```update_attribute``` method:
  - update single attribute
  - ***skip validation***

---

### 6.2 User validation

#### 6.2.1 A validity test

General method:

- create *valid* model object
- set one of attributes to *invalid* 
- test and say: *invalid*

How?

- create an *valid* model object in setup method
- test valid for him :)
- run ```rails test:models```

#### 6.2.2 Validating presence

- create new test for model, set ```name``` attribute to blank, assert that is false
- *fail the test*
- update validation in ```User``` model
- get errors: ```user.errors.full_messages```
- failed test user can not be ```save```
- do it again with ```email``` attribute

#### 6.2.3 Length validation

- make length validation for name and email (similar to presence)

#### 6.2.4 Format validation

- Email format validation
  - General idea:
    - make a list of correct email address, make the test pass
    - make a list of incorrect email address, make the test *fail*
    - update format in model
    - make the test *pass*

#### 6.2.5 Uniqueness validation

- General idea
  - make ```duplicate_user``` with same email address as ```@user```
  - save ```@user```
  - test that ```duplicate_user``` is invalid 
  - test fail
  - update validation in user model
  - add case-sensitive functionality
- ***Problem***: not validate in db level :(
- Solution:
  - create index in email column
  - require that index must be unique
- How?
  - generate migration: ```rails generate migration add_index_to_users_email```
  - change in migration file
    - ```add_index :users, :email, unique: true```
      - ```add_index```: Rails method
      - ```users```: db table
      - ```:email```: column in table
      - ```unique: true```: enforce uniqueness
  - run ```rails db:migrate```
  - test still fail, why?
    - fixtures data violates uniqueness
    - but why other validation?
      - fixtures data doesn't get through validations
    - => delete contents in ```users.yml``` file
- ***Another problem***:
  - Some db are case_sensitive (F@gmail.com !== f.gmail.com) => !== index
- Solution: downcase email before save
- How?:
  - use **before_save** *callback*
    - *callback* is method which will be called at particular point in lifecycle of AR obj
- *Ex*: write test to ensure that email is downcased --> *done*

---

### 6.3 Adding secure password

General idea

- require each user has a password
- hash this password, store hash vesion in db

How to authenticate?

- use sunmitting password
- hash that submitted password
- compare that hashed version with db (db store hashed version)
- match => log him in :)

#### 6.3.1 A hashed password

- include ```has_secure_password``` method in model
  - abililty to save ```password_digest``` to db (```password_digest``` is hashed version of password)
  - pair of *virtual attributes*: ```password``` and ```password_confirmation```, with *presence* and *match* validation
    - *virtual attributes*: attributes exist in model object, not correspond to db column
  - ```authenticate``` method (return user when password is correct, ```false``` if passowrd is incorrect)
- for ```has_secure_password``` work, model must have ```password_digest``` (type ```string```) attributes
- how?
  - using migration:
    - ```rails generate migration add_password_digest_to_users password_digest:string```
    - *should end with ```to_users```: for Rails to constructs migration for ```users``` table*
    - ```rails db:migrate``` or ```rdm```
    - add ```bcrypt``` gem (for ```has_secure_password``` to hash)


#### 6.3.2 User has secure password

- Add ```has_secure_password``` to ```User``` model
- test -> fail, why?
  - ```has_secure_password``` need ```password``` and ```password_confirmation``` virtual attribues, but our test obj doesn't have
  - *Solution*: add ```password``` and ```pasword_confirmation``` fields to the test obj
  - test pass :)

#### 6.3.3 Minimum password standard

- Write test for password
  - not blank
  - longer than 6 chars
- fail test
- update in ```User``` model
- pass test :)

#### 6.3.4 Create and authenticating user

- Create user in db (not in ```sandbox``` mode)
- get some user then assign to ```user``` var
- ```user.authenticate([password goes here])```, test the result:
  - right password: object
    - to convert to boolean: add ```!!```
  - wrong password: ```false```

---

### 6.4 Conclusion




