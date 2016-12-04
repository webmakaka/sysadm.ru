---
layout: page
title: Load balancing with NGINX
permalink: /linux/containers/coreos/Introduction_to_CoreOS/Deploying_A_DatabaseBacked_Web_Application/Load_Balancing_With_NGINX_confd/
---


# Load balancing with NGINX


core-01


 **$ vi nginx.service**


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
    ExecStartPre=-/usr/bin/docker pull rosskukulinski/nginx-proxy
    ExecStart=/usr/bin/docker run --name %p-%i \
          -h %H \
          -p ${COREOS_PUBLIC_IP}:80:80 \
          rosskukulinski/nginx-proxy
    ExecStop=-/usr/bin/docker kill %p-%i
    ExecStop=-/usr/bin/docker rm %p-%i

    [X-Fleet]
    Global=true

<br/>

    $ fleetctl submit nginx.service
    $ fleetctl start nginx.service

<br/>

    $ fleetctl list-units

<br/>

    http://172.17.8.101/
    http://172.17.8.102/
    http://172.17.8.103/
