---
layout: page
title: Getting Started with CoreOS [29 Nov 2016, ENG]
permalink: /linux/containers/coreos/Getting_Started_with_CoreOS/
---


# Getting Started with CoreOS [29 Nov 2016, ENG]


https://github.com/sysadm-ru/CoreOS-GettingStarted


Начало как всегда мало интересное.
Да и остальная часть мало, чем отличается от материалов курса [O’Reilly Media / Infinite Skills] Introduction to CoreOS Training Video [2015, ENG].

Поэтому сразу с посделеней главы, где уже приводится окончательный вариант кластера.



<br/>

### Discovering Services in CoreOS Cluster



**Vagrantfile**

https://github.com/sysadm-ru/CoreOS-GettingStarted/blob/master/Sidekicks/VagrantFile



    $update_channel='stable'



**user-data**

https://github.com/sysadm-ru/CoreOS-GettingStarted/blob/master/Sidekicks/user-data


https://discovery.etcd.io/new?size=3


    $ vi user-data

    discovery: https://discovery.etcd.io/89e341b6012e47d7e6654eea7b882418

    $ vagrant box update

    $ vagrant up


<br/>

    $ vagrant status
    Current machine states:

    core-01                   running (virtualbox)
    core-02                   running (virtualbox)
    core-03                   running (virtualbox)


<br/>

    $ vagrant ssh core-01

<br/>

    $ fleetctl list-machines
    MACHINE		IP		METADATA
    422b9f3c...	172.17.8.102	-
    5e438c8c...	172.17.8.101	-
    f94d7902...	172.17.8.103	-

<br/>

    $ fleetctl list-units   
    UNIT	MACHINE	ACTIVE	SUB


<br/>

<div align="center">
    <img src="//files.sysadm.ru/img/linux/containers/coreos/getting_started_with_coreos/pic4.png" border="0" alt="fleetctl">
</div>

<br/>



<br/>

    $ vi hello@.service

    [Unit]
    Description=Hello World template unit
    After=docker.service
    Requires=docker.service  

    [Service]
    ExecStartPre=-/usr/bin/docker kill %p-%i
    ExecStartPre=-/usr/bin/docker rm %p-%i
    ExecStart=/usr/bin/docker run --name %p-%i busybox /bin/sh -c "while true; do echo Hello World; sleep 1; done"
    ExecStop=/usr/bin/docker stop %p-%i
    Restart=on-failure


<br/>

    $ fleetctl start hello@1
    $ fleetctl start hello@2

<br/>

    $ vi hello-discovery@.service


    [Unit]
    Description=Announce Hello Service
    BindsTo=hello@%i.service
    After=hello@%i.service

    [Service]
    ExecStart=/bin/sh -c "while true; do etcdctl set /services/hello/%i $(docker inspect -f '{{.NetworkSettings.IPAddress}}' hello-%i) --ttl 60;sleep 45;done"
    ExecStop=/usr/bin/etcdctl rm /services/hello/svc@%i

    [X-Fleet]
    MachineOf=hello@%i.service



<br/>

    $ fleetctl start hello-discovery@1
    $ fleetctl start hello-discovery@2

<br/>


    $ fleetctl list-units  
    UNIT				MACHINE				ACTIVE	SUB
    hello-discovery@1.service	422b9f3c.../172.17.8.102	active	running
    hello-discovery@2.service	5e438c8c.../172.17.8.101	active	running
    hello@1.service			422b9f3c.../172.17.8.102	active	running
    hello@2.service			5e438c8c.../172.17.8.101	active	running

<br/>

    $ etcdctl ls /services/hello
    /services/hello/2
    /services/hello/1

<br/>

    $ etcdctl get /services/hello/1
    172.18.0.2

<br/>

    $ etcdctl get /services/hello/2
    172.18.0.2

<br/>

    $ fleetctl destroy hello-discovery@2


ждем 60 сек

    $ etcdctl ls /services/hello
    /services/hello/1



<br/>

### Using Flannel


<br/>

<div align="center">
    <img src="//files.sysadm.ru/img/linux/containers/coreos/getting_started_with_coreos/pic5.png" border="0" alt="fleetctl">
</div>

<br/>


<br/>

<div align="center">
    <img src="//files.sysadm.ru/img/linux/containers/coreos/getting_started_with_coreos/pic6.png" border="0" alt="fleetctl">
</div>

<br/>


<div align="center">
    <img src="//files.sysadm.ru/img/linux/containers/coreos/getting_started_with_coreos/pic7.png" border="0" alt="fleetctl">
</div>

<br/>


    $ vagrant destroy

<br/>

https://discovery.etcd.io/new?size=3


**user-data**

https://github.com/sysadm-ru/CoreOS-GettingStarted/blob/master/Flannel/user-data

    $ vi user-data

    discovery: https://discovery.etcd.io/d31c2d50f7febe70ef36e2db9db387ef


    $ vagrant up


<br/>


Повторяем, чтобы было:

    $ fleetctl list-units  
    UNIT				MACHINE				ACTIVE	SUB
    hello-discovery@1.service	422b9f3c.../172.17.8.102	active	running
    hello-discovery@2.service	5e438c8c.../172.17.8.101	active	running
    hello@1.service			422b9f3c.../172.17.8.102	active	running
    hello@2.service			5e438c8c.../172.17.8.101	active	running


<br/>

    $ etcdctl get /services/hello/1
    10.1.68.2

<br/>

    $ etcdctl get /services/hello/2
    10.1.40.2

<br/>

    core@core-01 ~ $ ping 10.1.40.2
    PING 10.1.40.2 (10.1.40.2) 56(84) bytes of data.
    64 bytes from 10.1.40.2: icmp_seq=1 ttl=61 time=1.06 ms
    64 bytes from 10.1.40.2: icmp_seq=2 ttl=61 time=0.968 ms
    ^C
    --- 10.1.40.2 ping statistics ---
    2 packets transmitted, 2 received, 0% packet loss, time 1001ms
    rtt min/avg/max/mdev = 0.968/1.014/1.060/0.046 ms

<br/>

    core@core-01 ~ $ ping 10.1.68.2
    PING 10.1.68.2 (10.1.68.2) 56(84) bytes of data.
    64 bytes from 10.1.68.2: icmp_seq=1 ttl=64 time=0.084 ms
    64 bytes from 10.1.68.2: icmp_seq=2 ttl=64 time=0.082 ms
    64 bytes from 10.1.68.2: icmp_seq=3 ttl=64 time=0.075 ms


<br/>

### Database and Web



**rethinkdb-discovery@.service**

https://github.com/sysadm-ru/CoreOS-GettingStarted/blob/master/EndToEnd/rethinkdb-discovery%40.service

<br/>

    $ vi rethinkdb-discovery@.service


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

https://github.com/sysadm-ru/CoreOS-GettingStarted/blob/master/EndToEnd/rethinkdb%40.service


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



<br/>

    $ fleetctl start rethinkdb-discovery@1
    $ fleetctl start rethinkdb@1


<br/>

    $ fleetctl list-units
    UNIT				MACHINE				ACTIVE	SUB
    rethinkdb-discovery@1.service	b2a2bade.../172.17.8.103	active	running
    rethinkdb@1.service		b2a2bade.../172.17.8.103	active	running



http://172.17.8.103:18080 - админка rethingdb

1 инстанс


    $ etcdctl ls /services/rethinkdb
    /services/rethinkdb/172.17.8.103

<br/>

    $ etcdctl ls /services/rethinkdb | xargs -I {} etcdctl get {}
    172.17.8.103

<br/>

    $ etcdctl ls /services/rethinkdb | xargs -I {} etcdctl get {} | sed s/^/"--join "/ | sed s/$/":29015"/
    --join 172.17.8.103:29015

<br/>

    $ etcdctl ls /services/rethinkdb | xargs -I {} etcdctl get {} | sed s/^/"--join "/ | sed s/$/":29015"/ | tr "\n" " "
    --join 172.17.8.103:29015 core@core-01 ~ $

<br/>

    $ fleetctl start rethinkdb-discovery@2
    $ fleetctl start rethinkdb@2

    $ fleetctl start rethinkdb-discovery@3
    $ fleetctl start rethinkdb@3


<br/>

http://172.17.8.103:4001/v2/keys/services/rethinkdb - rethinkdb api

<br/>

Проект отсюда можно взять:

https://github.com/sysadm-ru/CoreOS-GettingStarted/tree/master/EndToEnd

<br/>

Но за нас уже все сделано:

<br/>

https://github.com/sysadm-ru/CoreOS-GettingStarted/blob/master/EndToEnd/helloweb-discovery%40.service

<br/>

    $ vi helloweb-discovery@.service

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

https://github.com/sysadm-ru/CoreOS-GettingStarted/blob/master/EndToEnd/helloweb%40.service

    $ vi helloweb@.service


    [Unit]
    BindsTo=helloweb-discovery@%i.service
    After=helloweb-discovery@%i.service

    [Service]
    EnvironmentFile=/etc/environment
    TimeoutStartSec=10m
    ExecStartPre=-/usr/bin/docker kill %p-%i
    ExecStartPre=-/usr/bin/docker rm %p-%i
    ExecStartPre=/usr/bin/docker pull jolson88/coreos-gettingstarted-web
    ExecStart=/bin/sh -c '/usr/bin/docker run --name %p-%i  \
        -p 8080:8080                                        \
        -e COREOS_PRIVATE_IPV4=${COREOS_PRIVATE_IPV4}       \
        jolson88/coreos-gettingstarted-web'
    ExecStop=/usr/bin/docker stop %p-%i
    Restart=always

    [X-Fleet]
    MachineOf=helloweb-discovery@%i.service


<br/>

    $ fleetctl start helloweb-discovery@1.service
    $ fleetctl start helloweb-discovery@2.service
    $ fleetctl start helloweb-discovery@3.service

<br/>

    $ fleetctl start helloweb@1.service
    $ fleetctl start helloweb@2.service
    $ fleetctl start helloweb@3.service

<br/>

Подключился к приложению:

http://172.17.8.103:8080/greeting


<br/>

### Load Balancing


В общем понадобился docker контейнер с 2 файлами.

https://github.com/sysadm-ru/CoreOS-GettingStarted/blob/master/EndToEnd/lb/gen-nginx-conf.sh


https://github.com/sysadm-ru/CoreOS-GettingStarted/blob/master/EndToEnd/lb/lb-startup.sh


https://github.com/sysadm-ru/CoreOS-GettingStarted/blob/master/EndToEnd/lb/Dockerfile


Но за нас уже все сделали и мы можем ничего не делать. Ура! Вот бы он еще за работу за меня ходил!

    $ vi hellolb@.service

    [Unit]
    Requires=docker.service
    After=docker.service

    [Service]
    EnvironmentFile=/etc/environment
    TimeoutStartSec=10m
    ExecStartPre=-/usr/bin/docker kill %p-%i
    ExecStartPre=-/usr/bin/docker rm %p-%i
    ExecStartPre=/usr/bin/docker pull jolson88/coreos-gettingstarted-lb
    ExecStart=/bin/sh -c '/usr/bin/docker run --name %p-%i  \
        -p 8888:8888                                        \
        -e COREOS_PRIVATE_IPV4=${COREOS_PRIVATE_IPV4}       \
        jolson88/coreos-gettingstarted-lb'
    ExecStop=/usr/bin/docker stop %p-%i
    Restart=always

    [X-Fleet]
    Conflicts=hellolb@%i.service


<br/>


    $ fleetctl start hellolb@1.service


http://172.17.8.102:8888/greeting

Меняется веб сервер к которому подключаемся.


    $ fleetctl ssh hellolb@1

у меня ошибка, т.к. я ssh не насраивал для таких задач.


    $ fleetctl list-machines
    MACHINE		IP		METADATA
    400b64e9...	172.17.8.101	-
    5ac0c403...	172.17.8.102	-
    b2a2bade...	172.17.8.103	-


Оказалось у меня на 2-м coreos сервере.
Впринципе узнал методом тыка


    $ docker exec -it hellolb-1 bash

<br/>

    # cat /opt/lb/nginx.conf
    events {
        worker_connections 4096;
    }

    http {
        upstream backend {
            server 172.17.8.103:8080;
            server 172.17.8.101:8080;
        }
        server {
            listen 8888;
            location / {
                proxy_pass http://backend;
            }
        }
    }


Третий сервис у меня завалился и не поднялся. Да и хрен бы с ним.
