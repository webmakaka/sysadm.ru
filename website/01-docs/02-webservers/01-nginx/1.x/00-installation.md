---
layout: page
title: Инсталляция Nginx 1.X сервера на Ubuntu из пакетов
description: Инсталляция Nginx 1.X сервера на Ubuntu из пакетов
keywords: Инсталляция Nginx 1.X сервера на Ubuntu из пакетов
permalink: /adm/webservers/nginx/1.x/ubuntu/installation/
---

# Инсталляция Nginx 1.X сервера на Ubuntu из пакетов

Делаю:  
19.08.2018

<br/>

    $ sudo su -
    # apt-get update -y && apt-get upgrade -y
    # apt-get install -y vim curl links wget

<br/>

    # cd /tmp/
    # wget http://nginx.org/keys/nginx_signing.key
    # apt-key add nginx_signing.key

<br/>

    # cd /etc/apt/sources.list.d/

<br/>

    # vi nginx.list

<br/>
Добавляю в конец файла
<br/>

**Если (Bionic)**

```shell
deb http://nginx.org/packages/ubuntu/ bionic nginx
deb-src http://nginx.org/packages/ubuntu/ bionic nginx

```

<br/>

**Если (xenial)**

```shell
deb http://nginx.org/packages/ubuntu/ xenial nginx
deb-src http://nginx.org/packages/ubuntu/ xenial nginx

```

<br/>

    # apt-get update
    # apt-get install -y nginx

<br/>

    # service nginx restart
    # service nginx status
    # systemctl enable nginx

<br/>

    # nginx -v
    nginx version: nginx/1.14.0

<br/>

    # curl -I http://localhost
    HTTP/1.1 200 OK
    Server: nginx/1.14.0
    Date: Fri, 04 May 2018 02:44:21 GMT
    Content-Type: text/html
    Content-Length: 612
    Last-Modified: Tue, 17 Apr 2018 15:22:36 GMT
    Connection: keep-alive
    ETag: "5ad6113c-264"
    Accept-Ranges: bytes

<br/>

    # links http://localhost

<br/>

    # netstat -aln | grep 80
    tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN

<br/>

По умолчанию работает на порту 80. Поменять можно:

    # vi /etc/nginx/conf.d/default.conf
