---
layout: page
title: Ruby on Rails
permalink: /linux/dev/ruby-on-rails/
---

### С использованием rbenv

    # yum install -y \
    which \
    tar \
    curl \
    openssl-devel \
    git \
    gcc


Создаю пользователя developer.

    # useradd \
    -d /home/developer \
    -m developer

<br/>

    # passwd developer

<br/>

    # mkdir /rails_projects
    # chown -R developer /rails_projects/


<br/>

    # su - developer

<br/>

    $ cd ~/
    $ git clone git://github.com/sstephenson/rbenv.git .rbenv
    $ echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
    $ echo 'eval "$(rbenv init -)"' >> ~/.bashrc
    $ exec $SHELL

<br/>

    $ git clone git://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
    $ echo 'export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"' >> ~/.bashrc
    $ exec $SHELL

<br/>

    $ rbenv install --list

<br/>

    $ rbenv install 2.2.3

<br/>

    $ rbenv versions
    2.2.3


<br/>

    $ rbenv global 2.2.3
    $ ruby -v



<br/>

### RubyGems

    $ gem -v
    2.4.5.1


<br/>

    $ gem list

<br/>

    $ gem update --system



<br/>

### Rails

    $ echo "gem: --no-ri --no-rdoc" > ~/.gemrc

<br/>

    $ gem install bundler
    $ rbenv rehash
    $ bundle -v
    Bundler version 1.7.4

<br/>

    $ gem install rails --no-ri --no-rdoc
    $ rbenv rehash
    $ rails -v


<br/>

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

### WebServer (WEBrick)

    $ rails server

http://192.168.1.21:3000/


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


http://192.168.1.21:3000/demo/index


<br/>

### Setup Ruby On Rails on Ubuntu 14.10 Utopic Unicorn

https://gorails.com/setup/ubuntu/14.10
