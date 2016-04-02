---
layout: page
title: Настройка Nginx как proxy сервера
permalink: /linux/webservers/nginx/1.8/debian/jessie/proxy/
---


Устанавливаю также как <a href="http://sysadm.ru/linux/webservers/nginx/debian/installation/">здесь</a>

На 192.168.1.202 работает webserver и принимает и корректно обрабатывает запросы на обращение по адресу sysadm.ru на порту 8080.

    # vi /etc/hosts
    192.168.1.202 webserver

<br/>


    # cd /etc/nginx/conf.d

    # rm -rf *


<br/>

    # vi /etc/nginx/conf.d/sysadm.ru.conf

    upstream webserver  {
          server webserver:8080; #Webserver
    }

    server {
        listen     80;
        server_name sysadm.ru;

        ## send request back to apache1 ##
        location / {
         proxy_pass  http://webserver;
         proxy_next_upstream error timeout invalid_header http_500 http_502 http_503 http_504;
         proxy_redirect off;
         proxy_buffering off;
         proxy_set_header        Host            $host;
         proxy_set_header        X-Real-IP       $remote_addr;
         proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
        }
    }


<br/>


    # service nginx restart

<br/>

    # curl sysadm.ru:80
    <h1>Hello, sysadm.ru</h1>
