# Review Rails tutorial

## Table of contents

- [Chapter 3](#chapter-3)
- [Chapter 4](#chapter-4)

## Chapter 3

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

---



## Chapter 4

