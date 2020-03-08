---
layout: page
title: MongoDB инсталляция в Ubuntu 18.04.1
description: MongoDB инсталляция в Ubuntu 18.04.1
keywords: linux, ubuntu, mongodb, install
permalink: /databases/mongodb/install/ubuntu/
---

# MongoDB инсталляция в Ubuntu 18.04.1

**Актуальная версия MongoDB 4.2**


Делаю!  
19.08.2018

```
// mongodb repo
# echo 'deb http://downloads-distro.mongodb.org/repo/ubuntu-upstart dist 10gen' > /etc/apt/sources.list.d/mongodb.list
# apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 9ECBEC467F0CEB10

# apt update
# apt install -y mongodb

# mongo --version
MongoDB shell version v3.6.3
git version: 9586e557d54ef70f9ca4b43c26892cd55257e1a5
OpenSSL version: OpenSSL 1.1.0g  2 Nov 2017
allocator: tcmalloc
modules: none
build environment:
    distarch: x86_64
    target_arch: x86_64


# service mongodb restart
# service mongodb status
# systemctl enable mongodb

```

<br/>

### MongoDB инсталляция в Ubuntu 16.04 (Еще 1 вариант)

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
