---
layout: page
title: Инсталляция Nginx как web сервер из пакетов в Centos 7
description: Инсталляция Nginx как web сервер из пакетов в Centos 7
keywords: Инсталляция Nginx как web сервер из пакетов в Centos 7
permalink: /server/linux/webserver/nginx/1.8/centos/7/webserver/
---

# Инсталляция Nginx как web сервер из пакетов в Centos 7

    # yum update -y

<br/>

    # rpm -Uvh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm

<br/>

    # yum install -y nginx \
    vim \
    links \
    curl

<br/>

    # systemctl start nginx

<br/>

    # curl -I http://localhost:80
    HTTP/1.1 200 OK
    Server: nginx/1.8.1
    Date: Sat, 02 Apr 2016 10:44:17 GMT
    Content-Type: text/html
    Content-Length: 612
    Last-Modified: Tue, 26 Jan 2016 15:03:41 GMT
    Connection: keep-alive
    ETag: "56a78acd-264"
    Accept-Ranges: bytes

<br/>

    # links http://localhost
