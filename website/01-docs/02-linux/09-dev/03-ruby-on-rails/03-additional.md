---
layout: page
title: Какая-то херня осталась от предыдущих копаний
permalink: /linux/dev/ruby-on-rails/additionals/
---


# Какая-то херня осталась от предыдущих копаний


### MySQL

    # yum install -y mysql-devel mysql mysql-server

<br/>

    $ gem install mysql2

-----

<br/>

### Sqlite


-- В Centos 6.5

    # yum install -y sqlite-devel

-- В Ubuntu 14.04

    # apt-get install libsqlite3-dev


<br/>

### CREATING a PROJECT

    $ cd /rails_projects

<br/>

    // mysql
    $ rails new myApp1 -d mysql

<br/>

    // sqlite
    $ rails new myApp1 -d sqlite3

<br/>

    $ cd myApp1/

<br/>

    $ vi Gemfile

<br/>

Оставили только rails и mysql2 / sqlite3



<br/>

### rails console

    # yum install readline-devel

в GemFIle добавляю

    gem 'rb-readline'

<br/>

    $ bundle install


<br/>

    $ vi ./config/database.yml

Указываем логин и пароль для подключения к базе.
Комментируем название базы если имеется.
database: #myApp1_development



<br/>

### Generatin a Controller and View


    $ rails generate controller demo index

<br/>

    $ vi app/controllers/demo_controller.rb

<br/>

    class DemoController < ApplicationController

        layout false

      def index
      end
    end


http://localhost:3000/demo/index
