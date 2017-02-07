---
layout: page
title: CoreOS > Example 01
permalink: /linux/containers/coreos/example/01/
---


# CoreOS > Example 01

**Vagrantfile и user-data**

https://github.com/sysadm-ru/Native-Docker-Clustering

<br/>

https://discovery.etcd.io/new?size=6


    $ vi user-data

Заменяю сгенерированным ключом.

    discovery: https://discovery.etcd.io/89e341b6012e47d7e6654eea7b882418


<br/>

    $ vagrant box update


<br/>

    $ vagrant up

<br/>

    $ vagrant status
    Current machine states:

    core-01                   running (virtualbox)
    core-02                   running (virtualbox)
    core-03                   running (virtualbox)
    core-04                   running (virtualbox)
    core-05                   running (virtualbox)
    core-06                   running (virtualbox)


<br/>

    $ vagrant ssh core-01


<br/>

    $ fleetctl list-machines
    MACHINE		IP		METADATA
    047ef507...	10.0.11.5	-
    104a924a...	10.0.12.5	-
    2ccc7711...	10.0.14.5	-
    3c89f9a9...	10.0.15.5	-
    8df586c8...	10.0.16.5	-
    b9048ab8...	10.0.13.5	-


<br/>

### Базы данных


    $ vi rethinkdb-announce@.service

<br/>

    [Unit]
    Description=Announce RethinkDB %i service

    [Service]
    EnvironmentFile=/etc/environment
    ExecStart=/bin/sh -c "while true; do etcdctl set /services/rethinkdb/rethinkdb-%i ${COREOS_PUBLIC_IPV4} --ttl 60; sleep 45; done"
    ExecStop=/usr/bin/etcdctl rm /services/rethinkdb/rethinkdb-%i

    [X-Fleet]
    X-Conflicts=rethinkdb-announce@*.service



<br/>

    $ vi rethinkdb@.service

<br/>

    [Unit]
    Description=RethinkDB %i service
    After=docker.service
    BindsTo=rethinkdb-announce@%i.service

    [Service]
    EnvironmentFile=/etc/environment
    TimeoutStartSec=0
    ExecStartPre=-/usr/bin/docker kill rethinkdb-%i
    ExecStartPre=-/usr/bin/docker rm rethinkdb-%i
    ExecStartPre=-/usr/bin/mkdir -p /home/core/docker-volumes/rethinkdb
    ExecStartPre=/usr/bin/docker pull marley/coreos-rethinkdb:latest
    ExecStart=/bin/sh -c '/usr/bin/docker run --name rethinkdb-%i   \
     -p ${COREOS_PUBLIC_IPV4}:8080:8080                        \
     -p ${COREOS_PUBLIC_IPV4}:28015:28015                      \
     -p ${COREOS_PUBLIC_IPV4}:29015:29015                      \
     marley/coreos-rethinkdb:latest rethinkdb --bind all \
     --canonical-address ${COREOS_PUBLIC_IPV4}                 \
     $(/usr/bin/etcdctl ls /services/rethinkdb |               \
         xargs -I {} /usr/bin/etcdctl get {} |                 \
         sed s/^/"--join "/ | sed s/$/":29015"/ |              \
         tr "\n" " ")'

    ExecStop=/usr/bin/docker stop rethinkdb-%i

    [X-Fleet]
    X-ConditionMachineOf=rethinkdb-announce@%i.service


<br/>

    $ fleetctl submit *

    $ fleetctl list-unit-files
    UNIT				HASH	DSTATE		STATE		TARGET
    rethinkdb-announce@.service	3f7611a	inactive	inactive	-
    rethinkdb@.service		96c6e09	inactive	inactive	-


<br/>

    $ fleetctl start rethinkdb@1 rethinkdb-announce@1
    $ fleetctl start rethinkdb@2 rethinkdb-announce@2


<br/>

    $ fleetctl list-units
    UNIT				MACHINE			ACTIVE	SUB
    rethinkdb-announce@1.service	047ef507.../10.0.11.5	active	running
    rethinkdb-announce@2.service	104a924a.../10.0.12.5	active	running
    rethinkdb@1.service		047ef507.../10.0.11.5	active	running
    rethinkdb@2.service		104a924a.../10.0.12.5	active	running


<br/>

    $ curl 10.0.11.5:8080

Все ок. получил контент от сервера баз данных.


<br/>

### Web Сервера


    $ etcdctl get /services/rethinkdb/rethinkdb-1
    10.0.11.5

    $ etcdctl get /services/rethinkdb/rethinkdb-2
    10.0.12.5

<br/>


    $ cd /tmp/
    $ git clone --depth=1 https://github.com/sysadm-ru/Introduction_To_CoreOS
    $ cd Introduction_To_CoreOS/Chapter\ 5/todo-angular-express/

<br/>

В файле config.js

'172.17.8.101' меняю на '10.0.11.5'    

<br/>

    $ docker build --rm -t marley/coreos-app .


<br/>

    $ docker images
    REPOSITORY                TAG                 IMAGE ID            CREATED             SIZE
    marley/coreos-app         latest              54533dca8c65        8 minutes ago       736.1 MB
    marley/coreos-rethinkdb   latest              ee254ccee514        8 weeks ago         181.8 MB
    iojs                      2.2                 2a1868f3dfd8        20 months ago       703.8 MB

<br/>

    $ docker login

Ранее в веб интерфейсе создано репо.

    $ docker push marley/coreos-app

<br/>

    $ cd ~

<br/>

    $ vi todo@.service

<br/>

    [Unit]
    Description=ToDo Service

    Requires=docker.service
    Requires=todo-sk@%i.service
    After=docker.service

    [Service]
    EnvironmentFile=/etc/environment
    User=core

    Restart=always
    TimeoutStartSec=0
    ExecStartPre=-/usr/bin/docker kill %p-%i
    ExecStartPre=-/usr/bin/docker rm %p-%i
    ExecStartPre=/usr/bin/docker pull marley/coreos-app
    ExecStart=/usr/bin/docker run --name %p-%i \
          -h %H \
          -p ${COREOS_PUBLIC_IPV4}:3000:3000 \
          -e INSTANCE=%p-%i \
          marley/coreos-app
    ExecStop=-/usr/bin/docker kill %p-%i
    ExecStop=-/usr/bin/docker rm %p-%i

    [X-Fleet]
    Conflicts=todo@*.service


<br/>

    $ vi todo-sk@.service

<br/>


    [Unit]
    Description=ToDo Sidekick
    Requires=todo@%i.service

    After=docker.service
    After=todo@%i.service
    BindsTo=todo@%i.service

    [Service]
    EnvironmentFile=/etc/environment
    User=core
    Restart=always
    TimeoutStartSec=0
    ExecStart=/bin/bash -c '\
    while true; do \
     port=$(docker inspect --format=\'\' todo-%i); \
     curl -sf ${COREOS_PUBLIC_IPV4}:$port/ > /dev/null 2>&1; \
     if [ $? -eq 0 ]; then \
       etcdctl set /services/todo/todo-%i ${COREOS_PUBLIC_IPV4}:$port --ttl 10; \
     else \
       etcdctl rm /services/todo/todo-%i; \
     fi; \
     sleep 5; \
     done'

    ExecStop=/usr/bin/etcdctl rm /services/todo/todo-%i

    [X-Fleet]
    MachineOf=todo@%i.service


<br/>

    $ fleetctl submit todo*


<br/>

    $ fleetctl list-unit-files
    UNIT				HASH	DSTATE		STATE		TARGET
    rethinkdb-announce@.service	3f7611a	inactive	inactive	-
    rethinkdb-announce@1.service	3f7611a	launched	launched	047ef507.../10.0.11.5
    rethinkdb-announce@2.service	3f7611a	launched	launched	104a924a.../10.0.12.5
    rethinkdb@.service		96c6e09	inactive	inactive	-
    rethinkdb@1.service		96c6e09	launched	launched	047ef507.../10.0.11.5
    rethinkdb@2.service		96c6e09	launched	launched	104a924a.../10.0.12.5
    todo-sk@.service		64bb9b6	inactive	inactive	-
    todo@.service			2372aa6	inactive	inactive	-


<br/>

    $ fleetctl start todo@{3..4} todo-sk@{3..4}

<br/>

    $ fleetctl list-units
    UNIT				MACHINE			ACTIVE	SUB
    rethinkdb-announce@1.service	047ef507.../10.0.11.5	active	running
    rethinkdb-announce@2.service	104a924a.../10.0.12.5	active	running
    rethinkdb@1.service		047ef507.../10.0.11.5	active	running
    rethinkdb@2.service		104a924a.../10.0.12.5	active	running
    todo-sk@3.service		3c89f9a9.../10.0.15.5	active	running
    todo-sk@4.service		2ccc7711.../10.0.14.5	active	running
    todo@3.service			3c89f9a9.../10.0.15.5	active	running
    todo@4.service			2ccc7711.../10.0.14.5	active	running


<br/>

    $ curl 10.0.15.5:3000


Все ок. получил контент приложения от вебсервера.

Скорее всего на этом шаге ошибка!  
В etcd дожна быть запись /services/todo/todo-%i, а ее нет.

    $ etcdctl ls --recursive
    /coreos.com
    /coreos.com/updateengine
    /coreos.com/updateengine/rebootlock
    /coreos.com/updateengine/rebootlock/semaphore
    /coreos.com/network
    /coreos.com/network/config
    /coreos.com/network/subnets
    /coreos.com/network/subnets/10.1.30.0-24
    /coreos.com/network/subnets/10.1.57.0-24
    /coreos.com/network/subnets/10.1.65.0-24
    /coreos.com/network/subnets/10.1.2.0-24
    /coreos.com/network/subnets/10.1.61.0-24
    /coreos.com/network/subnets/10.1.84.0-24
    /services
    /services/rethinkdb
    /services/rethinkdb/rethinkdb-1
    /services/rethinkdb/rethinkdb-2
    /services/todo


    $ echo ${COREOS_PUBLIC_IPV4}
    10.0.11.5

    $ echo $(docker inspect --format=\'\' todo-%i);
    Error: No such image, container or task: todo-%i
    []


Похоже, нужно запускать на машине, где запущен контейнер с именем todo-%i  
И подобрать команду для получения порта контейнера с сервисом по имени todo-%i  

Может???

    $ docker inspect --format="{{(index (index .NetworkSettings.Ports \"3000/tcp\") 0).HostPort}}" todo-4


Чего-то всеравно не видно, где бы это дальше использовалось.

<br/>

### Proxy

    $ cd /tmp/Introduction_To_CoreOS/Chapter\ 5/nginx-proxy/

<br/>

    $ vi confd-watch

заменил

export HOST_IP=${HOST_IP:-172.17.8.101}

на

export HOST_IP=${HOST_IP:-10.0.15.5}


<br/>

    $ docker build --rm -t marley/coreos-nginx .


<br/>

    $ docker images
    REPOSITORY                TAG                 IMAGE ID            CREATED             SIZE
    marley/coreos-nginx       latest              957faace8b64        11 seconds ago      159.3 MB
    marley/coreos-app         latest              54533dca8c65        41 minutes ago      736.1 MB
    marley/coreos-rethinkdb   latest              ee254ccee514        8 weeks ago         181.8 MB
    nginx                     1.9.3               ea4b88a656c9        19 months ago       132.8 MB
    iojs                      2.2                 2a1868f3dfd8        20 months ago       703.8 MB


<br/>


    $ docker login

Ранее в веб интерфейсе создано репо.

    $ docker push marley/coreos-nginx

<br/>

    $ cd ~

<br/>

    $ vi nginx.service


<br/>


    [Unit]
    Description=Nginx Proxy

    Requires=docker.service
    After=docker.service
    After=etcd2.service
    Requires=etcd2.service

    [Service]
    EnvironmentFile=/etc/environment
    User=core

    Restart=always
    TimeoutStartSec=0
    ExecStartPre=-/usr/bin/docker kill %p-%i
    ExecStartPre=-/usr/bin/docker rm %p-%i
    ExecStartPre=-/usr/bin/etcdctl mkdir /services/todo
    ExecStartPre=-/usr/bin/docker pull marley/coreos-nginx
    ExecStart=/usr/bin/docker run --name %p-%i \
          -h %H \
          -p ${COREOS_PUBLIC_IP}:80:80 \
          marley/coreos-nginx
    ExecStop=-/usr/bin/docker kill %p-%i
    ExecStop=-/usr/bin/docker rm %p-%i

    [X-Fleet]
    Global=true

<br/>

    $ fleetctl submit nginx.service

<br/>

    $ fleetctl list-unit-files
    UNIT				HASH	DSTATE		STATE		TARGET
    nginx.service			c3f2a9b	inactive	-		global
    rethinkdb-announce@.service	3f7611a	inactive	inactive	-
    rethinkdb-announce@1.service	3f7611a	launched	launched	047ef507.../10.0.11.5
    rethinkdb-announce@2.service	3f7611a	launched	launched	104a924a.../10.0.12.5
    rethinkdb@.service		96c6e09	inactive	inactive	-
    rethinkdb@1.service		96c6e09	launched	launched	047ef507.../10.0.11.5
    rethinkdb@2.service		96c6e09	launched	launched	104a924a.../10.0.12.5
    todo-sk@.service		64bb9b6	inactive	inactive	-
    todo-sk@3.service		64bb9b6	launched	launched	3c89f9a9.../10.0.15.5
    todo-sk@4.service		64bb9b6	launched	launched	2ccc7711.../10.0.14.5
    todo@.service			2372aa6	inactive	inactive	-
    todo@3.service			2372aa6	launched	launched	3c89f9a9.../10.0.15.5
    todo@4.service			2372aa6	launched	launched	2ccc7711.../10.0.14.5



<br/>

    $ fleetctl start nginx.service

<br/>

    $ fleetctl list-units


Получил

### 404 Not Found
