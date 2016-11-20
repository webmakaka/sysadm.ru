---
layout: page
title: Load balancing with HAPROXY & CONFD
permalink: /linux/virtual/coreos/load-balancing-with-haproxy-and-confd/
---


### Load balancing with HAPROXY & CONFD


core-01


    $ fleetctl stop nginx.service
    $ fleetctl unload nginx.service


<br/>

 **$ vi haproxy.service**


<br/>

    [Unit]
    Description=haproxy Proxy

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
    ExecStartPre=-/usr/bin/docker pull rosskukulinski/haproxy-proxy
    ExecStart=/usr/bin/docker run --name %p-%i \
          -h %H \
          -p ${COREOS_PUBLIC_IP}:80:80 \
          rosskukulinski/haproxy-proxy
    ExecStop=-/usr/bin/docker kill %p-%i
    ExecStop=-/usr/bin/docker rm %p-%i

    [X-Fleet]
    Global=true


<br/>

    $ fleetctl submit haproxy.service
    $ fleetctl start haproxy.service

<br/>

    $ fleetctl list-units

<br/>

    http://172.17.8.101/
    http://172.17.8.102/
    http://172.17.8.103/
