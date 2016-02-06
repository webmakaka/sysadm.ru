---
layout: page
title: Перенос форума в контейнеры Docker
permalink: /linux/virtual/docker/odba/
---


Имеем форум работающий в VirtualBox.

Хочу перенести его в контейнеры docker (да еще и на хост с coreos).
Нужен также docker compose, git. И для импорта / экспорта phpmyadmin.


Для начала на bitbucket делаю приватное репо с php скриптами и дампом базы данных.


Раз мы используем docker, то я пожалуй возьму уже подготовленное окружение.


    $ docker pull debian
    $ docker pull mysql

https://hub.docker.com/_/mysql/



    $ docker run -i -t -p 80:8080 --name deb debian /bin/bash

Все я в домике

Устанавливаю nginx как здесь.  
sysadm.ru/linux/webservers/nginx/debian/installation/


    # cd /projects/sysadm.ru/
    # git clone --depth=1 https://github.com/phpmyadmin/phpmyadmin
