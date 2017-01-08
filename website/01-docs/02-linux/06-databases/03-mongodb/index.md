---
layout: page
title: MongoDB инсталляция в Centos 6.X
permalink: /linux/databases/mongodb/
---

# MongoDB инсталляция в Centos 6.X

### Mongo 3.X

    # vi /etc/yum.repos.d/mongodb-3x.repo

<br/>
Добавляем
<br/>

    [MongoDB]
    name=MongoDB Repository
    baseurl=http://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.0/x86_64/
    gpgcheck=0
    enabled=1

<br/>

    # yum install -y mongodb-org

    # chkconfig mongod on
    # service mongod restart

    # mongo --version

<br/>

### Mongo 2.X

    # vi /etc/yum.repos.d/mongodb-2x.repo

{% highlight text %}

[10gen]
name=10gen Repository
baseurl=http://downloads-distro.mongodb.org/repo/redhat/os/x86_64
gpgcheck=0
enabled=1

{% endhighlight %}



<br/>

    # yum install -y mongo-10gen mongo-10gen-server

    # chkconfig mongod on
    # service mongod restart

    # mongo


### Конфиг

/etc/mongod.conf



### Подключиться к MongoDB

    $ mongo

При необходимости с параметрами

    $ mongo --host localhost --port 49153

___


**При разработке и если данные не нужны можно отключить журналирования для более быстрой работы:**

    mkdir -p /data/db
    echo 'mongod --nojournal --dbpath=/data/db' > mongod-start
    chmod a+x mongod-start
    ./mongod-start



<br/>

### GUI Клиент для подключения к MongoDB в Ubuntu - RoboMongo

<br/>

### Ошибки:

Решил как обычно подключиться к базе mongodb на облачном хостинге mongolab:

    $ mongo ds027744.mongolab.com:27744/comevents -u user1 -p user1

    MongoDB shell version: 2.6.11
    connecting to: ds027744.mongolab.com:27744/comevents
    2015-10-02T20:21:11.912+0000 Error: 18 { ok: 0.0, errmsg: "auth failed", code: 18 } at src/mongo/shell/db.js:1292
    exception: login failed


    # mongo ds027744.mongolab.com:27744/comevents -u user1 -p user1

В общем обновили на mongolab версию базы и клиент не хочет цепляться к более новой версии базы. Требуется его обновление. Я работал на тестовом окружении и просто переставил 3-ю версию, после чего заработало.

    # mongo ds027744.mongolab.com:27744/comevents -u user1 -p user1
    MongoDB shell version: 3.0.6
    connecting to: ds027744.mongolab.com:27744/comevents
