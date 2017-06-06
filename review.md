# Review Rails tutorial

## Table of contents

- [Chapter 3](#chapter-3)
- [Chapter 4](#chapter-4)
- [Chapter 5](#chapter-5)
- [Chapter 6](#chapter-6)
- [Chapter 7](#chapter-7)
- [Chapter 8](#chapter-8)
- [Chapter 9](#chapter-9)
- [Chapter 10](#chapter-10)

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

---

## Chapter 8

- Destination
  - login / logout functionality
  - maintain login state until browser closed
  - customize site / implement authorization based on login status
- Techniques:
  - ```session``` method: create temporary session (expire when browser close)
  - using just ```named routes```
  - ```rails routes``` for listing all routes

---

### 8.1 Sessions

- HTTP is stateless protocol
  - next request is unable to use any information from previos requests
- => We must use session
- In Rails, most common techniques for implementing session is *cookies*
  - *cookies* is a *small text* on user's browser, persist from one page to the next => store information, retrieve logged user in db
- General idea:
  - visit login page	
    - render a form for ***new session***
  - logging in
    - create ***session***
  - loggin out
    - destroy ***session***
- ```Sessions``` resource will use *cookies* for persist data
- => We need to create 
  - ```Sessions``` controller
  - login form
  - relevant controller actions

---

#### 8.1.1 Session controller

- General idea (```Sessions``` controller)
  - login form: handled by ```new``` action
    - logging in: send ```POST``` to ```create``` action
    - logging out: send ```DELETE``` to ```destroy``` action
- How?
  - create new ```Sessions``` controller: ```rg controller Sessions new```
  - just using named routes => update in ```routes.rb``` file
    - update generated test file
    - run ```rails routes``` for listing all routes

#### 8.1.2 Login form

- Problems
  - When login error, we can't use error_messages partial, why?
    - because this just work with Active Record object
    - => Solution: use ```flash``` :)
  - When creating form, we don't have ```Session``` model, so can't use ```form_for``` like before
    - because Rails don't know action of the form should be ```POST``` and the URL of that form
    - => Solution: use ```form_for``` with name of *resource* and corresponding URL:
      - ```form_for(:session, url: login_path)```

#### 8.1.3 Finding and authenticating a user

- handle submit form: ```create``` action
- read ```session``` hash from ```params``` hash, use ```find_by``` and ```authentication``` method to authenticate user

#### 8.1.4 Rendering flash message

- if login error => ```flash[:danger]```
- problem: ```flash``` message don't go
  - because ```render``` is not count at the request

#### 8.1.5 A flash test

- Strategy:
  - Visit the login path.
  - Verify that the new sessions form renders properly.
  - Post to the sessions path with an invalid `params` hash.
  - Verify that the new sessions form gets re-rendered and that a flash message appears.
  - Visit another page (such as the Home page).
  - Verify that the flash message *doesn’t* appear on the new page.
- => test *fail*
- => solution to get ```flash``` away: use ```flash.now```

---

#### 8.2 Logging in

- Make ```SessionHelper``` module available to our controllers
  - in ```application_controller.rb```, add ```include SessionsHelper```

#### 8.2.1 ```log_in``` method

- use ```session``` method from Rails
- save ```user.id``` to this ```session```

#### 8.2.2 ```current_user```

- Problem: ```User.find(session[:user_id])``` often raise ```exception```
  - => solution: use ```User.find_by```	(return ```nil```)
    - problem: this will hit db many times
      - => solution: store result in instance variable:
        - ```@current_user ||= User.find_by(id: session[:user_id])```

#### 8.2.3 Change layout links

- in ```session_helper```, define ```logged_in?``` method (check the ```current_user``` is ```nil``` or not)
- use ```logged_in?``` in template => easy
  - include bootstrap js in ```application.js```

#### 8.2.4 Tesing layout changes

- Strategy:
  - Visit the login path.
  - Post valid information to the sessions path.
  - Verify that the login link disappears.
  - Verify that a logout link appears
  - Verify that a profile link appears.

#### 8.2.5 Login upon signup

- ``` log_in @user``` after save to db

---

#### 8.3 Logout

- define ```log_out``` method from ```SessionHelper```
  - delete ```user_id``` from session
  - set ```current_user``` to ```nil```
- in ```destroy``` method from ```SessionsController```
  - ```log_out``` user
  - redirect to ```root_url```

---

### 8.4 Conclusion

----

## Chapter 9

Techniques:

- using ```cookies```

---

#### 9.1 Remember me

#### 9.1.1 Remember token and digest

- Strategy:
  - Create a random string of digits for use as a remember token.
  - Place the token in the browser cookies with an expiration date far in the future.
  - Save the hash digest of the token to the database.
  - Place an encrypted version of the user’s id in the browser cookies.
  - When presented with a cookie containing a persistent user id, find the user in the database using the given id, and verify that the remember token cookie matches the associated hash digest from the database.
- How?
  - add ```remember_digest``` to User's model
  - generate new token for user by ```SecureRandom.urlsafe_base64```
  - make ```user.remember``` method to:
    - associate remember token with user
    - save remember digest to db
    - problem:
      - ```User``` model don't have remember_token attribute
      - => solution: the same as the password => create a *virtual* ```remember_token``` attr
        - how?
          - use ```attr_accessor :remember_token```
    - assign ```remember_token``` with ```User.new_token``` method
    - update attribute to *hashed* version of remember_token

#### 9.1.2 Login with remembering

---

## Chapter 10

Techiques:

- ```before_action``` filter
- faking sample data
- pagination

---

### 10.1 Updating users

#### 10.1.1 Edit form

- handled by ```edit``` action
  - get user from db (by ```id``` from params)
  - solution: use ```@user = User.find_by```
  - create template using ```form_for```
    - use ```POST``` with *hidden* input field to fake ```PATCH``` request
  - how to know when *update* or *create* new?
    - when create form with ```form_for```, Rails check ```@user.new_record?```
      - ```true``` => ```POST```
      - ```false``` => ```PATCH```

#### 10.1.2 Unsuccessful edit

- handled by ```update``` method
  - read from ```params``` hash
    - if ```update_attributes``` with ```user_params``` (*strong params*)
      - ```flash```
      - ```redirect_to @user```
    - else
      - ```render 'edit'```

#### 10.1.3 Unsuccessful edit test

- using *integration test*
- strategy:
  - go to edit path
  - assert template
  - post invalid data
  - assert template

#### 10.1.4 Successful edit

- TDD: strategy
  - go to edit path
  - assert template
  - post valid data
  - assert ```flash```
  - assert ```redirect```
  - reload @user
  - assert equal
  - *set allow nil to password validation, because update password is not required*

---

### 10.2 Authorization

#### 10.2.1 Requiring logged-in users

- ```before_action```
  - arrange particular method to be called before some action
- to require login before ```edit``` and ```update``` action:
  - ```  before_action :logged_in_user, only: [:edit, :update]```
  - ```logged_in_user``` check if user is logged in or not, if not, ```flash``` and redirect
    - how?
      - use ```logged_in``` helper :)

#### 10.2.2 Requiring right user

- Problem: any logged user can change other's information
- => solution: use ```before_action``` *again*:
  - ```  before_action :correct_user,   only: [:edit, :update]```
  - ```correct_user```:
    - find ```@user``` by ```id``` in ```params```
    - redirect to ```root_url``` if ```@user``` !== ```current_user```

#### 10.2.3 Friendly fowarding

- Problem:
  - user should go to their intended destination
- => Solution:
  - store location of requested page somewhere
  - redirect to that location
- How?
  - create 2 methods:
    - ```store_location```
    - ```redirect_back_or```
  - in ```SessionHelper```, why?
    - because we'll save location in session
  - how to get url?
    - use ```request.original_url``` if ```request.get```
  - call ```store_location``` when login fail (```user_controller```)
  - call ```redirect_back_or user``` (```user``` is default) when login success (```sessions_controller```)


---

### 10.3 Show all users

#### 10.3.1 Users index

- Problem
  - Prevent unregistered user to view ```Users``` page
  - Solution
    - use ```before_action``` with ```index```, which ```logged_in_user```
- Show users
  - ```index``` action: ```@users = User.all```
  - make view template to show ```@users``` (use ```each``` method)

#### 10.3.2 Sample users

- Problem: too little user :(
  - Solution: fake more users
  - How?
    - use ```faker``` gem
    - in ```seed.rb``` file, write code for faking data
    - ```migrate:reset``` and ```seed``` db

#### 10.3.3 Pagination

- Problem: too many user, long scroll page
  - Solution: use ```will_paginate``` gem
  - use ```will_paginate``` in template view
  - update in ```index``` action

#### 10.3.4 Users index test

#### 10.3.5 Refactor

---

### 10.4 Delete users

#### 10.4.1 Admin users

- Problem: only admin can delete user

  - Solution:
    - add ```admin```, type ```boolean``` attribute to ```users``` table
  - How:
    - ```generate``` migration
    - ```migrate``` db
    - *strong params* will deny ```admin``` params if it has



#### 10.4.2 ```destroy``` action

- create link for ```delete```
- ```destroy``` action:
  - find user by ```id``` then delete him
  - ```flash```
  - ```redirect_to users_url```
  - create ```before_action``` ```admin_user``` filter for ```destroy```

#### 10.4.3 ```destroy``` test

---

### 10.5 Conclusion

