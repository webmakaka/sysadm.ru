---
layout: page
title: Инсталляция Nginx 1.8 сервера на Debian из пакетов
permalink: /linux/webservers/nginx/1.8/debian/jessie/installation/
---

### Инсталляция Nginx 1.8 сервер на Debian из пакетов


<br/>

    # apt-get update -y && apt-get upgrade -y
    # apt-get install -y vim curl links wget


<br/>

    # cd /tmp/
    # wget http://nginx.org/keys/nginx_signing.key
    # apt-key add nginx_signing.key

<br/>

    # vi /etc/apt/sources.list

<br/>
Добавляю в конец файла
<br/>

    deb http://nginx.org/packages/debian/ jessie nginx
    deb-src http://nginx.org/packages/debian/ jessie nginx

<br/>

    # apt-get update
    # apt-get install -y nginx

<br/>

    # service nginx restart
    # service nginx status

<br/>

    # curl -I http://localhost
    HTTP/1.1 200 OK
    Server: nginx/1.8.1
    Date: Sat, 02 Apr 2016 13:53:13 GMT
    Content-Type: text/html
    Content-Length: 612
    Last-Modified: Tue, 26 Jan 2016 15:24:47 GMT
    Connection: keep-alive
    ETag: "56a78fbf-264"
    Accept-Ranges: bytes


<br/>   

    # links http://localhost
