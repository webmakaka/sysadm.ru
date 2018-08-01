---
layout: page
title: MongoDB инсталляция в Ubuntu 16.04
permalink: /linux/servers/databases/mongodb/ubuntu/installation/
---

# MongoDB инсталляция в Ubuntu 16.04


Делаю!  
01.08.2018 

Мне нужна именно версия v3.6

Если нужна какая-нибудь другая, см. инстркцию, ссылка внизу.

    # apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 91FA4AD5

    # bash -c 'echo "deb http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.6 multiverse" > /etc/apt/sources.list.d/mongodb-org-3.6.list'


    -- Если нужно удалить репо
    # rm /etc/apt/sources.list.d/mongodb*.list


    # apt-get update

    # apt-get install -y mongodb-org

    # mongo --version
    MongoDB shell version v3.6.6
    git version: 6405d65b1d6432e138b44c13085d0c2fe235d6bd
    OpenSSL version: OpenSSL 1.0.2g  1 Mar 2016
    allocator: tcmalloc
    modules: none
    build environment:
        distmod: ubuntu1604
        distarch: x86_64
        target_arch: x86_64


https://askubuntu.com/questions/842592/apt-get-fails-on-16-04-installing-mongodb