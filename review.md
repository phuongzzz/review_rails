# Review Rails tutorial

## Table of contents

- [Chapter 3](#chapter-3)
- [Chapter 4](#chapter-4)

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

