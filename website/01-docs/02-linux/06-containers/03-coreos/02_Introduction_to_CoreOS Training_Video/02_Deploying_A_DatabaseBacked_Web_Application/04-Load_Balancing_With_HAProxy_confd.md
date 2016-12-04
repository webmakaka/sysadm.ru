---
layout: page
title: Load balancing with HAPROXY & CONFD
permalink: /linux/containers/coreos/Introduction_to_CoreOS/Deploying_A_DatabaseBacked_Web_Application/Load_Balancing_With_HAProxy_confd/
---


# Load balancing with HAPROXY & CONFD


<br/>

 **$ vi haproxy.service**


<br/>

{% highlight text %}

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

{% endhighlight %}


<br/>

    $ fleetctl submit haproxy.service
    $ fleetctl start haproxy.service

<br/>

    $ fleetctl list-units

<br/>

    http://172.17.8.101/
    http://172.17.8.102/
    http://172.17.8.103/



    docker pull rosskukulinski/haproxy-proxy
    docker pull serg1i/haproxy-proxy
