---
layout: page
title: Deploying RethinkDB Database
permalink: /linux/containers/coreos/Introduction_to_CoreOS/Deploying_A_DatabaseBacked_Web_Application/Deploying_RethinkDB_Database/
---


# [O’Reilly Media / Infinite Skills] Introduction to CoreOS Training Video [2015, ENG] : Deploying A DatabaseBacked Web Application : Deploying RethinkDB Database



### DataBase Layer (RethinkDB) 8080 port - database admin dashboard


    $ ssh-add ~/.vagrant.d/insecure_private_key
    $ vagrant ssh core-01 -- -A


core-01

<br/>

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
    ExecStartPre=/usr/bin/docker pull rosskukulinski/rethinkdb:2.1.0_beta1
    ExecStart=/bin/sh -c '/usr/bin/docker run --name rethinkdb-%i   \
     -p ${COREOS_PUBLIC_IPV4}:8080:8080                        \
     -p ${COREOS_PUBLIC_IPV4}:28015:28015                      \
     -p ${COREOS_PUBLIC_IPV4}:29015:29015                      \
     rosskukulinski/rethinkdb:2.1.0_beta1 rethinkdb --bind all \
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


<br/>

    $ fleetctl list-unit-files
    UNIT				HASH	DSTATE		STATE		TARGET
    rethinkdb-announce@.service	3f7611a	inactive	inactive	-
    rethinkdb@.service		5698af1	inactive	inactive	-


<br/>

    $ fleetctl start rethinkdb@1 rethinkdb-announce@1
    $ fleetctl start rethinkdb@2 rethinkdb-announce@2




<br/>

    // лог
    $ fleetctl journal -f --lines=50 rethinkdb@1
    $ fleetctl journal -f --lines=50 rethinkdb@2

    $ fleetctl journal -f --lines=50 ethinkdb-announce@1
    $ fleetctl journal -f --lines=50 ethinkdb-announce@2

<br/>

Несколько минут ждал выполнения следующей команды ....

    $ fleetctl list-units
    UNIT				MACHINE				ACTIVE	SUB
    rethinkdb-announce@1.service	8d3af21c.../172.17.8.101	active	running
    rethinkdb-announce@2.service	b7d947a2.../172.17.8.103	active	running
    rethinkdb@1.service		8d3af21c.../172.17.8.101	active	running
    rethinkdb@2.service		b7d947a2.../172.17.8.103	active	running

<br/>

    http://172.17.8.101:8080/#servers
    http://172.17.8.103:8080/#servers
