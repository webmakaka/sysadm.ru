---
layout: page
title: Настройка работы своего сайта Nginx 1.6, PHP
description: Настройка работы своего сайта Nginx 1.6, PHP
keywords: Настройка работы своего сайта Nginx 1.6, PHP
permalink: /webservers/nginx/1.6/debian/jessie/mysite/
---

# Настройка работы своего сайта Nginx 1.6, PHP

### Настройка конфигов

Пусть при обращении по url обращается к локальному сайту.  
Добавляю в hosts.

    # vi /etc/hosts

    127.0.0.1 sysadm.ru

<br/>

    # cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.orig

<br/>

    # mkdir -p /projects/sysadm.ru/public/
    # mkdir -p /projects/sysadm.ru/logs/

<br/>

    # vi /etc/nginx/sites-available/sysadm.ru.config

    server {
        listen     *:8080;
        server_name sysadm.ru;
        access_log /projects/sysadm.ru/logs/access.log;
        error_log /projects/sysadm.ru/logs/error.log;
        root /projects/sysadm.ru/public;

        location / {
            index index.php index.html;
        }

        location ~ \.php$ {
             try_files $uri = 404;
             include fastcgi_params;
             fastcgi_pass  unix:/var/run/php5-fpm.sock;
             fastcgi_index index.php;

             fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
       }
    }

<br/>

### Добавление сайта во включенные

    # cd /etc/nginx/sites-enabled/

Мне не нужен default

    # rm default

    # ln -s /etc/nginx/sites-available/sysadm.ru.config
    # service nginx restart

<br/>

### Проверка

    # vi /projects/sysadm.ru/public/index.php

    <?php echo "<h1>Hello, sysadm.ru</h1>" ?>

<br/>

    # curl sysadm.ru:8080
    <h1>Hello, sysadm.ru</h1>

<br/>

    # cat /projects/sysadm.ru/logs/access.log
    127.0.0.1 - - [02/Apr/2016:12:09:07 +0000] "GET / HTTP/1.1" 200 43 "-" "curl/7.38.0"
    127.0.0.1 - - [02/Apr/2016:12:10:34 +0000] "GET / HTTP/1.1" 200 43 "-" "curl/7.38.0"
    127.0.0.1 - - [02/Apr/2016:12:10:44 +0000] "GET / HTTP/1.1" 200 37 "-" "curl/7.38.0"

<br/>

Если все OK. Можно из hosts убрать запись, что 127.0.0.1 это sysadm.ru
