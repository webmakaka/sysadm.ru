---
layout: page
title: Пример запуска coreos кластера с контейнерами docker, приложением, базой данных и прокси сервером
permalink: /linux/containers/coreos/example/02/
---


# Пример запуска coreos кластера с контейнерами docker, приложением, базой данных и прокси сервером

По материалам из видео курса:  

**Getting Started with CoreOS [29 Nov 2016, ENG]**

Советы по улучшению, принимаются.


<br/>

PS. Исходники с Dockerfile, можно взять здесь:

https://github.com/sysadm-ru/coreos-docker-examples/tree/master/02


Они могу понадобиться, если захочется собрать собственные контейнеры или просто посмотреть примеры.



<br/>

### Database and Web

**rethinkdb-discovery@.service**

https://github.com/sysadm-ru/coreos-docker-examples/blob/master/02/coreos-rethinkdb/rethinkdb-discovery%40.service

<br/>

    $ vi rethinkdb-discovery@.service

<br/>

    [Unit]
    Requires=docker.service
    After=docker.service

    [Service]
    EnvironmentFile=/etc/environment
    ExecStart=/bin/sh -c "while true; do etcdctl set /services/rethinkdb/${COREOS_PRIVATE_IPV4} ${COREOS_PRIVATE_IPV4} --ttl 60; sleep 45; done"
    ExecStop=/usr/bin/etcdctl rm /services/rethinkdb/${COREOS_PRIVATE_IPV4}

    [X-Fleet]
    Conflicts=rethinkdb-discovery@%i.service


<br/>

**rethinkdb@.service**

https://github.com/sysadm-ru/coreos-docker-examples/blob/master/02/coreos-rethinkdb/rethinkdb%40.service


<br/>

    $ vi rethinkdb@.service

<br/>

    [Unit]
    BindsTo=rethinkdb-discovery@%i.service
    After=rethinkdb-discovery@%i.service

    [Service]
    EnvironmentFile=/etc/environment
    TimeoutStartSec=10m
    ExecStartPre=-/usr/bin/docker kill %p-%i
    ExecStartPre=-/usr/bin/docker rm %p-%i
    ExecStartPre=-/usr/bin/mkdir -p /data/rethinkdb
    ExecStartPre=/usr/bin/docker pull rethinkdb
    ExecStart=/bin/sh -c '/usr/bin/docker run --name %p-%i  \
        -p 18080:18080               \
        -p 28015:28015               \
        -p 29015:29015               \
        -v /data/rethinkdb/:/data/                          \
        rethinkdb rethinkdb --bind all                \
        --http-port 18080                                   \
        --canonical-address ${COREOS_PRIVATE_IPV4}          \
        $(/usr/bin/etcdctl ls /services/rethinkdb |         \
            xargs -I {} /usr/bin/etcdctl get {} |           \
            sed s/^/"--join "/ | sed s/$/":29015"/ |        \
           tr "\n" " ")'
    ExecStop=/usr/bin/docker stop %p-%i
    Restart=on-failure

    [X-Fleet]
    MachineOf=rethinkdb-discovery@%i.service



<!-- <br/>

    $ fleetctl submit *

    $ fleetctl list-unit-files
    UNIT				HASH	DSTATE		STATE		TARGET
    rethinkdb-discovery@.service	683c1e5	inactive	inactive	-
    rethinkdb@.service		01c3289	inactive	inactive	- -->

<br/>

    $ fleetctl start rethinkdb-discovery@{5..7} rethinkdb@{5..7}


<br/>

    $ fleetctl list-units
    UNIT				MACHINE				ACTIVE	SUB
    rethinkdb-discovery@5.service	0044a05b.../172.17.8.104	active	running
    rethinkdb-discovery@6.service	33d8850a.../172.17.8.102	active	running
    rethinkdb-discovery@7.service	4765a2ce.../172.17.8.101	active	running
    rethinkdb@5.service		0044a05b.../172.17.8.104	active	running
    rethinkdb@6.service		33d8850a.../172.17.8.102	active	running
    rethinkdb@7.service		4765a2ce.../172.17.8.101	active	running


<br/>

    $ curl 172.17.8.101:18080


Можно подключиться браузером:  
http://172.17.8.101:18080


    $ etcdctl ls /services/rethinkdb
    /services/rethinkdb/172.17.8.104
    /services/rethinkdb/172.17.8.102
    /services/rethinkdb/172.17.8.101




<br/>

    $ etcdctl ls /services/rethinkdb | xargs -I {} etcdctl get {}
    172.17.8.101
    172.17.8.104
    172.17.8.102



<br/>

    $ etcdctl ls /services/rethinkdb | xargs -I {} etcdctl get {} | sed s/^/"--join "/ | sed s/$/":29015"/
    --join 172.17.8.104:29015
    --join 172.17.8.102:29015
    --join 172.17.8.101:29015



<br/>

    $ etcdctl ls /services/rethinkdb | xargs -I {} etcdctl get {} | sed s/^/"--join "/ | sed s/$/":29015"/ | tr "\n" " "
    --join 172.17.8.104:29015 --join 172.17.8.102:29015 --join 172.17.8.101:29015


<br/>

http://172.17.8.103:4001/v2/keys/services/rethinkdb - rethinkdb api

<br/>

### WebServer


<br/>

https://github.com/sysadm-ru/coreos-docker-examples/blob/master/02/coreos-gettingstarted-web/helloweb-discovery%40.service

<br/>

    $ vi helloweb-discovery@.service

<br/>

    [Unit]
    Requires=docker.service
    After=docker.service

    [Service]
    EnvironmentFile=/etc/environment
    ExecStart=/bin/sh -c "while true; do etcdctl set /services/web/${COREOS_PRIVATE_IPV4} ${COREOS_PRIVATE_IPV4} --ttl 60; sleep 45; done"
    ExecStop=/usr/bin/etcdctl rm /services/web/${COREOS_PRIVATE_IPV4}

    [X-Fleet]
    Conflicts=helloweb-discovery@%i.service


<br/>

https://github.com/sysadm-ru/coreos-docker-examples/blob/master/02/coreos-gettingstarted-web/helloweb%40.service

    $ vi helloweb@.service

<br/>

    [Unit]
    BindsTo=helloweb-discovery@%i.service
    After=helloweb-discovery@%i.service

    [Service]
    EnvironmentFile=/etc/environment
    TimeoutStartSec=10m
    ExecStartPre=-/usr/bin/docker kill %p-%i
    ExecStartPre=-/usr/bin/docker rm %p-%i
    ExecStartPre=/usr/bin/docker pull marley/coreos-gettingstarted-web
    ExecStart=/bin/sh -c '/usr/bin/docker run --name %p-%i  \
        -p 8080:8080                                        \
        -e COREOS_PRIVATE_IPV4=${COREOS_PRIVATE_IPV4}       \
        marley/coreos-gettingstarted-web'
    ExecStop=/usr/bin/docker stop %p-%i
    Restart=always

    [X-Fleet]
    MachineOf=helloweb-discovery@%i.service


<br/>

    $ fleetctl start helloweb-discovery@{2..4} helloweb@{2..4}

<br/>

    $ fleetctl list-units


<br/>


Подключился к приложению:

    $ curl http://172.17.8.103:8080/greeting
    "Hello, User! From 172.17.8.103"


<br/>

    // логи

    $ fleetctl journal -f --lines=100 helloweb@2.service


<br/>

### Load Balancing

https://github.com/sysadm-ru/coreos-docker-examples/blob/master/02/coreos-gettingstarted-lb/hellolb%40.service

    $ vi hellolb@.service

<br/>

    [Unit]
    Requires=docker.service
    After=docker.service

    [Service]
    EnvironmentFile=/etc/environment
    TimeoutStartSec=10m
    ExecStartPre=-/usr/bin/docker kill %p-%i
    ExecStartPre=-/usr/bin/docker rm %p-%i
    ExecStartPre=/usr/bin/docker pull marley/coreos-gettingstarted-lb
    ExecStart=/bin/sh -c '/usr/bin/docker run --name %p-%i  \
        -p 8888:8888                                        \
        -e COREOS_PRIVATE_IPV4=${COREOS_PRIVATE_IPV4}       \
        marley/coreos-gettingstarted-lb'
    ExecStop=/usr/bin/docker stop %p-%i
    Restart=always

    [X-Fleet]
    Conflicts=hellolb@%i.service


<br/>


    $ fleetctl start hellolb@1.service

<br/>

    $ fleetctl list-units
    UNIT				MACHINE				ACTIVE	SUB
    hellolb@1.service		dc247a24.../172.17.8.107	active	running
    helloweb-discovery@2.service	767bed22.../172.17.8.103	active	running
    helloweb-discovery@3.service	91b437a9.../172.17.8.105	active	running
    helloweb-discovery@4.service	bb98224a.../172.17.8.104	active	running
    helloweb@2.service		767bed22.../172.17.8.103	active	running
    helloweb@3.service		91b437a9.../172.17.8.105	active	running
    helloweb@4.service		bb98224a.../172.17.8.104	active	running
    rethinkdb-discovery@5.service	c539d45d.../172.17.8.102	active	running
    rethinkdb-discovery@6.service	47600f9c.../172.17.8.106	active	running
    rethinkdb-discovery@7.service	6d303877.../172.17.8.101	active	running
    rethinkdb@5.service		c539d45d.../172.17.8.102	active	running
    rethinkdb@6.service		47600f9c.../172.17.8.106	active	running
    rethinkdb@7.service		6d303877.../172.17.8.101	active	running

<br/>

    $ curl http://172.17.8.107:8888/greeting
    "Hello, User! From 172.17.8.103"


    core@core-01 ~ $ curl http://172.17.8.107:8888/greeting
    "Hello, User! From 172.17.8.105"

    core@core-01 ~ $ curl http://172.17.8.107:8888/greeting
    "Hello, User! From 172.17.8.104"


Меняется веб сервер к которому подключаемся.


<br/>

    $ fleetctl ssh hellolb@1

<br/>

    $ docker exec -it hellolb-1 bash

<br/>

    # cat /opt/lb/nginx.conf

<br/>

    events {
        worker_connections 4096;
    }

    http {
        upstream backend {
            server 172.17.8.103:8080;
            server 172.17.8.105:8080;
            server 172.17.8.104:8080;
        }
        server {
            listen 8888;
            location / {
                proxy_pass http://backend;
            }
        }
    }
