---
layout: page
title: Application Layer
permalink: /linux/containers/coreos/coreos-application-layer/
---


### Application Layer (Node.js) 3000 port


core-01


 **$ vi todo@.service**

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
    ExecStartPre=/usr/bin/docker pull rosskukulinski/angular-todo-rethinkdb
    ExecStart=/usr/bin/docker run --name %p-%i \
       -h %H \
       -p ${COREOS_PUBLIC_IPV4}:3000:3000 \
       -e INSTANCE=%p-%i \
       rosskukulinski/angular-todo-rethinkdb
    ExecStop=-/usr/bin/docker kill %p-%i
    ExecStop=-/usr/bin/docker rm %p-%i

    [X-Fleet]
    Conflicts=todo@*.service




 **$ vi todo-sk@.service**

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
     port=$(docker inspect --format=\'{{(index (index .NetworkSettings.Ports "3000/tcp") 0).HostPort}}\' todo-%i); \
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

    $ fleetctl start todo@{1..3} todo-sk@{1..3}

<br/>

    $ fleetctl list-units

<br/>

    $ fleetctl journal -f --lines=50 todo@1


<br/>

Поймал ошибку, которую не знаю как исправить.


    Jan 09 20:42:19 core-02 docker[9923]: Status: Downloaded newer image for rosskukulinski/angular-todo-rethinkdb:latest
    Jan 09 20:42:19 core-02 docker[9923]: docker.io/rosskukulinski/angular-todo-rethinkdb: this image was pulled from a legacy registry.  Important: This registry version will not be supported in future versions of docker.
    Jan 09 20:42:19 core-02 systemd[1]: Started ToDo Service.
    Jan 09 20:42:20 core-02 docker[10063]: Connecting to etcd on 172.17.42.1:4001
    Jan 09 20:42:23 core-02 docker[10063]: /src/config.js:14
    Jan 09 20:42:23 core-02 docker[10063]: var nodes = rethinks.body.node.nodes;
    Jan 09 20:42:23 core-02 docker[10063]: ^
    Jan 09 20:42:23 core-02 docker[10063]: TypeError: Cannot read property 'node' of undefined
    Jan 09 20:42:23 core-02 docker[10063]: at Object.<anonymous> (/src/config.js:14:26)
    Jan 09 20:42:23 core-02 docker[10063]: at Module._compile (module.js:426:26)
    Jan 09 20:42:23 core-02 docker[10063]: at Object.Module._extensions..js (module.js:444:10)
    Jan 09 20:42:23 core-02 docker[10063]: at Module.load (module.js:351:32)
    Jan 09 20:42:23 core-02 docker[10063]: at Function.Module._load (module.js:306:12)
    Jan 09 20:42:23 core-02 docker[10063]: at Module.require (module.js:361:17)
    Jan 09 20:42:23 core-02 docker[10063]: at require (module.js:380:17)
    Jan 09 20:42:23 core-02 docker[10063]: at Object.<anonymous> (/src/app.js:6:14)
    Jan 09 20:42:23 core-02 docker[10063]: at Module._compile (module.js:426:26)
    Jan 09 20:42:23 core-02 docker[10063]: at Object.Module._extensions..js (module.js:444:10)
    Jan 09 20:42:23 core-02 docker[10063]: at Module.load (module.js:351:32)
    Jan 09 20:42:23 core-02 docker[10063]: at Function.Module._load (module.js:306:12)
    Jan 09 20:42:23 core-02 docker[10063]: at Function.Module.runMain (module.js:467:10)
    Jan 09 20:42:23 core-02 docker[10063]: at startup (node.js:117:18)
    Jan 09 20:42:23 core-02 docker[10063]: at node.js:946:3
    Jan 09 20:42:24 core-02 systemd[1]: todo@1.service: Main process exited, code=exited, status=1/FAILURE
    Jan 09 20:42:24 core-02 docker[10159]: Error response from daemon: Cannot kill container todo-1: Container af2504f917be33f065a1025ab08a897601cbefee8992c0d4ff051813e3a3ee48 is not running
    Jan 09 20:42:24 core-02 docker[10173]: todo-1
    Jan 09 20:42:24 core-02 systemd[1]: Stopped ToDo Service.
    Jan 09 20:42:24 core-02 systemd[1]: todo@1.service: Unit entered failed state.
    Jan 09 20:42:24 core-02 systemd[1]: todo@1.service: Failed with result 'exit-code'.



<br/>

Далее по идее следует выполнять следующие команды:

<br/>

    $ etcdctl ls --recursive

<br/>

    $ etcdctl get /services/todo/todo-1

<br/>

    http://172.17.8.101:3000/
    http://172.17.8.102:3000/
    http://172.17.8.103:3000/
