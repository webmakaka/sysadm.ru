---
layout: page
title: MongoDB инсталляция в Centos 6.X
permalink: /linux/databases/mongodb/
---


    # vi /etc/yum.repos.d/mongodb.repo

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


Конфиг:  
/etc/mongod.conf


____


Подключиться к MongoDB

    $ mongo --host localhost --port 49153

___


**При разработке и если данные не нужны можно отключить журналирования для более быстрой работы:**

    mkdir -p /data/db
    echo 'mongod --nojournal --dbpath=/data/db' > mongod-start
    chmod a+x mongod-start
    ./mongod-start


____


    https://github.com/rsajob/docs/wiki/%D0%A3%D1%81%D1%82%D0%B0%D0%BD%D0%BE%D0%B2%D0%BA%D0%B0-MongoDb-%D0%BD%D0%B0-CentOS
    http://docs.mongodb.org/manual/tutorial/install-mongodb-on-red-hat-centos-or-fedora-linux/