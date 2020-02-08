---
layout: page
title: Собственный Docker Registry с самоподписанным TLS сертификатом
permalink: /linux/containers/docker/registry/self-signed-tls-security/
---

# Собственный Docker Registry с самоподписанным TLS сертификатом

Делаю:  
18.05.2019

<br/>

    # vi /etc/hosts

    127.0.0.1 registry.local

<br/>

    $ cd ~
    $ git clone https://bitbucket.org/sysadm-ru/self-hosted-docker-registry
    $ cd self-hosted-docker-registry/security/tls/
    $ docker-compose up

<!-- <br/>

https://192.168.0.11/v2/_catalog -->


<br/>

    // Забираем из запущенного контейнера сертификаты

    $ cd ~
    $ docker cp tls_secured_1:/certs .
    $ sudo cp certs/selfsigned.crt /usr/local/share/ca-certificates/
    $ sudo update-ca-certificates

<br/>

    $ sudo systemctl daemon-reload
    $ sudo systemctl restart docker

<br/>

    // Если есть желание проверить

    $ docker pull busybox
    $ docker image tag busybox registry.local/busybox
    $ docker image push registry.local/busybox

<br/>

    $ curl --insecure https://registry.local/v2/_catalog
    {"repositories":["busybox"]}


<br/>

**На клиенте**

<br/>

    # vi /etc/hosts

    192.168.0.11 registry.local

<br/>

Скопировал с помощью sftp файлы на клиент

<br/>

    $ sudo cp certs/selfsigned.crt /usr/local/share/ca-certificates/
    $ sudo update-ca-certificates

<!-- <br/>

    $ sudo systemctl daemon-reload
    $ sudo systemctl restart docker -->

<br/>

    $ docker pull mongo
    $ docker tag mongo registry.local/mongo
    $ docker push registry.local/mongo

<br/>

    $ curl --insecure https://registry.local/v2/_catalog
    {"repositories":["busybox","mongo"]}
