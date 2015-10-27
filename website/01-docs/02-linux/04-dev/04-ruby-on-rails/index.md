---
layout: page
title: Ruby on Rails
permalink: /linux/dev/ruby-on-rails/
---

<strong>rbenv</strong>

    # yum install -y \
    which \
    tar \
    curl \
    openssl-devel \
    git \
    gcc

<br/>

    # git config --global color.ui true


1) Создал пользователя developer.

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

=======================


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


============================================================
============================================================

    Setup Ruby On Rails on
    Ubuntu 14.10 Utopic Unicorn

https://gorails.com/setup/ubuntu/14.10

============================================================
============================================================

<strong>RubyGems</strong>

    $ gem -v
    2.4.5.1


<br/>

    $ gem list

<br/>

    $ gem update --system


============================================================

<strong>Rails</strong>

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


==========================================================

<strong>MySQL</strong>

    # yum install -y mysql-devel mysql mysql-server

<br/>

    $ gem install mysql2

-----

<strong>Sqlite</strong>


-- В Centos 6.5

    # yum install -y sqlite-devel

-- В Ubuntu 14.04

    # apt-get install libsqlite3-dev

==========================================================

<strong>CREATING a PROJECT</strong>

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


==========================================================


<strong>rails console</strong>

    # yum install readline-devel

в GemFIle добавляю

    gem 'rb-readline'

<br/>

    $ bundle install

==========================================================

    $ vi ./config/database.yml

Указываем логин и пароль для подключения к базе.
Комментируем название базы если имеется.
database: #myApp1_development

============================================================

<strong>WebServer (WEBrick)</strong>

    $ rails server

http://192.168.1.21:3000/

============================================================
============================================================


<strong>Generatin a Controller and View</strong>


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
