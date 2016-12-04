---
layout: page
title: Load balancing with NGINX
permalink: /linux/containers/coreos/Introduction_to_CoreOS/Deploying_A_DatabaseBacked_Web_Application/Load_Balancing_With_NGINX_confd/
---


# Load balancing with NGINX


confd:  
https://github.com/kelseyhightower/confd


<br/>

core-01


 **$ vi nginx.service**


<br/>


{% highlight text %}

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
ExecStartPre=-/usr/bin/docker pull rosskukulinski/nginx-proxy
ExecStart=/usr/bin/docker run --name %p-%i \
      -h %H \
      -p ${COREOS_PUBLIC_IP}:80:80 \
      rosskukulinski/nginx-proxy
ExecStop=-/usr/bin/docker kill %p-%i
ExecStop=-/usr/bin/docker rm %p-%i

[X-Fleet]
Global=true

{% endhighlight %}

<br/>

    $ fleetctl submit nginx.service
    $ fleetctl start nginx.service

<br/>

    $ fleetctl list-units
    UNIT				MACHINE				ACTIVE	SUB
    nginx.service			3408f7ab.../172.17.8.103	active	running
    nginx.service			b2ca4512.../172.17.8.101	active	running
    nginx.service			db577263.../172.17.8.102	active	running
    rethinkdb-announce@1.service	3408f7ab.../172.17.8.103	active	running
    rethinkdb-announce@2.service	b2ca4512.../172.17.8.101	active	running
    rethinkdb@1.service		3408f7ab.../172.17.8.103	active	running
    rethinkdb@2.service		b2ca4512.../172.17.8.101	active	running
    todo-sk@1.service		3408f7ab.../172.17.8.103	active	running
    todo-sk@2.service		db577263.../172.17.8.102	active	running
    todo-sk@3.service		b2ca4512.../172.17.8.101	active	running
    todo@1.service			3408f7ab.../172.17.8.103	active	running
    todo@2.service			db577263.../172.17.8.102	active	running
    todo@3.service			b2ca4512.../172.17.8.101	active	running


<br/>


    $ journalctl -f --lines -u nginx
    -- Logs begin at Mon 2016-11-21 19:42:58 UTC. --
    Dec 04 09:55:00 core-01 docker[27815]: 2016-12-04T09:55:00Z core-01 confd[9851]: FATAL cannot connect to etcd cluster: 172.17.42.1:4001
    Dec 04 09:55:00 core-01 docker[27815]: [nginx] waiting for confd to create initial nginx configuration.
    Dec 04 09:55:05 core-01 docker[27815]: 2016-12-04T09:55:05Z core-01 confd[9856]: INFO Backend set to etcd
    Dec 04 09:55:05 core-01 docker[27815]: 2016-12-04T09:55:05Z core-01 confd[9856]: INFO Starting confd
    Dec 04 09:55:05 core-01 docker[27815]: 2016-12-04T09:55:05Z core-01 confd[9856]: INFO Backend nodes set to 172.17.42.1:4001
    Dec 04 09:55:06 core-01 docker[27815]: 2016-12-04T09:55:06Z core-01 confd[9856]: FATAL cannot connect to etcd cluster: 172.17.42.1:4001
    Dec 04 09:55:06 core-01 docker[27815]: [nginx] waiting for confd to create initial nginx configuration.
    Dec 04 09:55:11 core-01 docker[27815]: 2016-12-04T09:55:11Z core-01 confd[9861]: INFO Backend set to etcd
    Dec 04 09:55:11 core-01 docker[27815]: 2016-12-04T09:55:11Z core-01 confd[9861]: INFO Starting confd
    Dec 04 09:55:11 core-01 docker[27815]: 2016-12-04T09:55:11Z core-01 confd[9861]: INFO Backend nodes set to 172.17.42.1:4001
    Dec 04 09:55:14 core-01 docker[27815]: 2016-12-04T09:55:14Z core-01 confd[9861]: FATAL cannot connect to etcd cluster: 172.17.42.1:4001
    Dec 04 09:55:14 core-01 docker[27815]: [nginx] waiting for confd to create initial nginx configuration.
    Dec 04 09:55:19 core-01 docker[27815]: 2016-12-04T09:55:19Z core-01 confd[9866]: INFO Backend set to etcd
    Dec 04 09:55:19 core-01 docker[27815]: 2016-12-04T09:55:19Z core-01 confd[9866]: INFO Starting confd
    Dec 04 09:55:19 core-01 docker[27815]: 2016-12-04T09:55:19Z core-01 confd[9866]: INFO Backend nodes set to 172.17.42.1:4001
    Dec 04 09:55:20 core-01 docker[27815]: 2016-12-04T09:55:20Z core-01 confd[9866]: FATAL cannot connect to etcd cluster: 172.17.42.1:4001
    Dec 04 09:55:20 core-01 docker[27815]: [nginx] waiting for confd to create initial nginx configuration.


<br/>

Ничего не заработало!


<br/>

core-01


    $ fleetctl stop nginx.service
    $ fleetctl unload nginx.service
    $ fleetctl destroy nginx.service


<br/>

Скорее всего, автор курса написал неправильно IP адрес.
Нужно вместо:


172.17.42.1 указать 172.17.8.101


Лень (пока) переделывать, какой-то чувак уже все сделал и достаточно заменить:

    pull rosskukulinski/nginx-proxy

на

    pull serg1i/nginx-proxy

<br/>

и повторить:


<br/>

После этого, у меня работает на 3-х машинах:

<br/>

    http://172.17.8.101/
    http://172.17.8.102/
    http://172.17.8.103/
