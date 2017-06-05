# Review Rails tutorial

## Table of contents

- [Chapter 3](#chapter-3)
- [Chapter 4](#chapter-4)
- [Chapter 5](#chapter-5)
- [Chapter 6](#chapter-6)
- [Chapter 7](#chapter-7)

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
- ```has_secure_password```

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

---

## Chapter 7

Techniques:

- ```debug``` method
- ```params``` variable
- ```sass mixin```
- Rest in Rails, ```resource``` in Rails
- How to retrive a user and display to view
- ```Digest::MD5::hexdigest```
- ```form_for```
- Strong params
- ```private``` method
- ```pluralize```
- ```user.errors.any?```
- ```count``` method
- ```assert_no_difference```
- ```redirect_to```

General ideas:

- use HTML form to submit user signup information
- use these information to create new user and save attributes to db
- after signup -> render profile page with newly user's information

### 7.1 Showing users

- Destination
  - make page to display user's information
  - just username and profile picture

#### 7.1.1 Debug Rails environments

- add ```<%= debug(params) if Rails.env.development? %>``` in ```application.html.erb``` file
  - 3 envs in Rails:
    - development
    - test
    - production

#### 7.1.2 A Users resource

- *Resource*: data can be *create, read, update, delete* via *HTTP* method
  - in REST, resource can be referenced using resource *name* and unique *id*:
    - ```/users/1```
- How to make a data become *resource*?
  - ```resources :users``` to ```routes.rb``` file
    - this will add all actions needed for Restful Users resource
    - add large number of named routes
  - visit ```/users/1``` -> error?
    - create ```show``` template for user
      - ```<%= @user.name %>, <%= @user.email %>```
    - how to get ```@user```?
      - in ```show``` action, define ```@user``` variable
        - ```@user = User.find(params[:id])```
        - ```params``` will return value based on URL

#### 7.1.3 Debugger

- ```debugger```

#### 7.1.4 Gravatar image and sidebar

- define ```gravatar_for``` helper
- any *helper* in any helper file are available to any *view*
- but we *should* define helper with appropriate controller

---

### 7.2 Signup form

- make signup page for our site

#### 7.2.1 ```form_for```

- Create ```User``` object for ```form-for```
  - ```@user = User.new```
  - go to ```new.html.erb```, create ```form``` using ```form_for```

#### 7.2.2 Signup form HTML

---

### 7.3 Unsuccessful signup

#### 7.3.1 A working form

- ```post``` action to ```/users``` will handled by ```create``` actions in controller
- how?
  - update ```create``` action in ```Users``` controller
- submitting some invalid data -> error -> inspecting params

#### 7.3.2 Strong params

- Problem: previous solution will pass *all* user's submitted data
  - some additional data can be submitted to user
- => We can not let user submit all data 
- Solution
  - Using *strong params*
    - specify which params are *required* and *permitted*
  - In this case
    - *require*: ```params``` hash must have ```user``` hash
    - permitted: ```name, email, password, password_confirmation```
    - => it will return
      - ```params``` with *permitted* attributes
      - ```error``` if ```:user``` is missing
- How?
  - create ```user_params``` method which return correct hash
    - make this method ```private```
  - use this method in ```create``` action

#### 7.3.3 Signup error messages

- Problem: show error message if it has
- Solution:
  - create ```error_messages``` partials
  - ```if @user.errors.any?``` -> render 
  - ```pluralize``` method
    - ```pluralize(5, "error")```
      - 1st arg: number
      - 2nd arg: singular
  - ```count``` method

---

#### 7.3.4 Test for invalid submit

- Using integration test
- General idea:
  - verify that clicking signup button doesn't create new user when form contains errors
  - how to know if it created?
    - => check the User.count before and after post :)
- How?
  - generate test file: ```rails generate integration_test users_signup```
  - get ```signup_path```
  - ```POST``` some data to ```users_path```
  - use ```assert_no_difference``` method to check before and after creating ```User```
  - test pass :)
  - *Ex*: write test for error divs :D


### 7.4 Successful signup

General idea:

- save the user
- success -> user's information will be written to db
- redirect the browser to show user's profile

#### 7.4.1 The finished signup form

- Problem: submit -> freeze form
- Why?
  - rails default behavior is render corresponding view
  - but here, there's not a view template for create action
- Solution
  - Redirect to another page when creation is successfully
  - ```redirect_to @user```
    - Rails infers ```redirect_to @user``` to ```user_url(@user)```

#### 7.4.2 The flash

- ```flash[:success] = "Welcome to the Sample App!"```
- display message on the first page after redirect, how?
  - iterate through flash (flash is a hash)
  - insert all relevant messages into the site layout
- => the rest is easy!

#### 7.4.3 The first signup

- reset db: ```rails db:migrate:reset```
- restart server
- test by hand for signup new user

#### 7.4.4 Test for valid submission

- General idea
  - Similar to invalid submission: check the db's contents
  - different: 
    - use ```assert_different```
    - ```follow_redirect!```
    - test for ```flash```

---

### 7.5 Professional-grade deployment (omitted)











