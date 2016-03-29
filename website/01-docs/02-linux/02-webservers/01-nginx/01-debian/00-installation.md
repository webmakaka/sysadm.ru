---
layout: page
title: Инсталляция Nginx сервер на Debian из пакетов
permalink: /linux/webservers/nginx/debian/installation/
---

### Инсталляция Nginx сервер на Debian из пакетов


    # vi /etc/hosts

    127.0.0.1 sysadm.ru


<br/>

    # apt-get update
    # apt-get install -y vim curl
    # apt-get install -y nginx


<br/>

    # service nginx restart
    # service nginx status

<br/>

    # curl -I http://localhost:80  
    HTTP/1.1 200 OK
    Server: nginx/1.6.2
    Date: Sat, 06 Feb 2016 15:30:54 GMT
    Content-Type: text/html
    Content-Length: 867
    Last-Modified: Sat, 06 Feb 2016 13:34:31 GMT
    Connection: keep-alive
    ETag: "56b5f667-363"
    Accept-Ranges: bytes


### Настройка конфигов

    # cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.orig

    # mkdir -p /projects/sysadm.ru
    # mkdir -p /var/log/nginx/sysadm.ru

<br/>

    # vi /etc/nginx/sites-available/sysadm.ru.config

    server {
        listen     *:8080;
        server_name sysadm.ru;
        access_log /var/log/nginx/sysadm.ru/access.log;
        error_log /var/log/nginx/sysadm.ru/error.log;
        root /projects/sysadm.ru;

        location / {
            index index.html index.htm index.php;
        }
    }


<br/>


### Добавление сайта во включенные

    # cd /etc/nginx/sites-enabled/
    # ln -s /etc/nginx/sites-available/sysadm.ru.config
    # service nginx restart


<br/>

### Проверка

    # vi /projects/sysadm.ru/index.html

    <h1>Hello, sysadm.ru</h1>

<br/>

    # curl sysadm.ru:8080
    <h1>Hello, sysadm.ru</h1>

<br/>

    # cat /var/log/nginx/sysadm.ru/access.log;
    127.0.0.1 - - [06/Feb/2016:16:24:49 +0000] "HEAD / HTTP/1.1" 200 0 "-" "curl/7.38.0"
    127.0.0.1 - - [06/Feb/2016:16:24:56 +0000] "GET / HTTP/1.1" 200 27 "-" "curl/7.38.0"


<br/>  

Если все OK. Можно из hosts убрать запись, что 127.0.0.1 это sysadm.ru
