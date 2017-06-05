# Review Rails tutorial

## Table of contents

- [Chapter 3](#chapter-3)
- [Chapter 4](#chapter-4)
- [Chapter 5](#chapter-5)

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



