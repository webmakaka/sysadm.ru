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
    $ docker run -i -t -p 80:8080 --name deb debian /bin/bash

Все я в домике

    # apt-get update

    # apt-get install -y nginx



















<br/>

    $ vi Dockerfile

<br/>

    FROM centos:7
    MAINTAINER "you" <your@email.here>
    ENV container docker
    RUN (cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == systemd-tmpfiles-setup.service ] || rm -f $i; done); \
    rm -f /lib/systemd/system/multi-user.target.wants/*;\
    rm -f /etc/systemd/system/*.wants/*;\
    rm -f /lib/systemd/system/local-fs.target.wants/*; \
    rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
    rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
    rm -f /lib/systemd/system/basic.target.wants/*;\
    rm -f /lib/systemd/system/anaconda.target.wants/*;
    VOLUME [ "/sys/fs/cgroup" ]
    CMD ["/usr/sbin/init"]

<br/>

    $ docker build --rm -t local/c7-systemd .



### Example systemd enabled app container

    $ vi Dockerfile

<br/>

    FROM local/c7-systemd
    RUN yum -y install httpd; yum clean all; systemctl enable httpd.service
    EXPOSE 80
    CMD ["/usr/sbin/init"]

<br/>

    $ docker build --rm -t local/c7-systemd-httpd .


### Running a systemd enabled app container


    $ docker run -ti -v /sys/fs/cgroup:/sys/fs/cgroup:ro -p 80:80 local/c7-systemd-httpd



https://hub.docker.com/r/million12/nginx-php/





    $ docker pull million12/nginx-php

    $ docker run -d -p=80:8080 --name=web million12/nginx-php


















https://hub.docker.com/r/jdeathe/centos-ssh-apache-php/



    $ docker run -d \
      --name apache-php.app-1.1.1 \
      -p 8080:80 \
      --env "SERVICE_UNIT_APP_GROUP=app-1" \
      --env "SERVICE_UNIT_LOCAL_ID=1" \
      --env "SERVICE_UNIT_INSTANCE=1" \
      --env "APACHE_SERVER_NAME=app-1.local" \
      --env "DATE_TIMEZONE=UTC" \
      -v /var/www/app \
      jdeathe/centos-ssh-apache-php:latest


Все, apache сервер уже настроен.

Можно подключиться к хосту по адресу http://<docker-host>:8080


    $ docker exec -it apache-php.app-1.1.1 bash


На хосте:


    # yum install -y git
    # cd /var/www/app/public_html
    # rm -rf *


// Требуется работа с php версией > 5.3. Поэтому и не из главного бранча беру phpmyadmin.

    # git clone --depth=1 -b MAINT_4_0_10 https://github.com/phpmyadmin/phpmyadmin


    http://<docker-host>:8080/phpmyadmin/


    # yum install -y php-mysql php-mbstring
    # apachectl restart
