---
layout: page
title: Deploying Web Application
permalink: /linux/containers/coreos/Introduction_to_CoreOS/Deploying_A_DatabaseBacked_Web_Application/Deploying_Web_Application/
---


# [O’Reilly Media / Infinite Skills] Introduction to CoreOS Training Video [2015, ENG] : Deploying A DatabaseBacked Web Application : Deploying Web Application


### Deploying Web Application


core-01


    $ fleetctl list-machines
    MACHINE		IP		METADATA
    3408f7ab...	172.17.8.103	-
    b2ca4512...	172.17.8.101	-
    db577263...	172.17.8.102	-


<br/>


    $ fleetctl list-units
    UNIT				MACHINE				ACTIVE	SUB
    rethinkdb-announce@1.service	3408f7ab.../172.17.8.103	active	running
    rethinkdb-announce@2.service	b2ca4512.../172.17.8.101	active	running
    rethinkdb@1.service		3408f7ab.../172.17.8.103	active	running
    rethinkdb@2.service		b2ca4512.../172.17.8.101	active	running


<br/>

    $ etcdctl get /services/rethinkdb/rethinkdb-1
    172.17.8.103

    $ etcdctl get /services/rethinkdb/rethinkdb-2
    172.17.8.101


<br/>


 **$ vi todo@.service**


{% highlight text %}

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

{% endhighlight %}



<br/>

 **$ vi todo-sk@.service**


{% highlight text %}

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


{% endhighlight %}

<br/>

    $ fleetctl submit todo*

<br/>


    // $ fleetctl start todo@1 todo-sk@1

    $ fleetctl start todo@{1..3} todo-sk@{1..3}



<br/>

    $ fleetctl list-units
    UNIT				MACHINE				ACTIVE	SUB
    rethinkdb-announce@1.service	3408f7ab.../172.17.8.103	active	running
    rethinkdb-announce@2.service	b2ca4512.../172.17.8.101	active	running
    rethinkdb@1.service		3408f7ab.../172.17.8.103	active	running
    rethinkdb@2.service		b2ca4512.../172.17.8.101	active	running
    todo-sk@1.service		b2ca4512.../172.17.8.101	failed	failed
    todo-sk@2.service		db577263.../172.17.8.102	failed	failed
    todo-sk@3.service		3408f7ab.../172.17.8.103	failed	failed
    todo@1.service			b2ca4512.../172.17.8.101	failed	failed
    todo@2.service			db577263.../172.17.8.102	failed	failed
    todo@3.service			3408f7ab.../172.17.8.103	failed	failed



<br/>

    // $ fleetctl unload todo@{1..3} todo-sk@{1..3}


<br/>

    // $ fleetctl list-unit-files


<br/>

    // $ fleetctl destroy todo@{1..3}.service
    // $ fleetctl destroy todo-sk@{1..3}.service

<br/>

    $ fleetctl journal -f --lines=50 todo@1


<br/>

Поймал ошибку, которую не знаю как исправить.


    Dec 04 01:31:36 core-01 docker[14455]: /src/config.js:14
    Dec 04 01:31:36 core-01 docker[14455]: var nodes = rethinks.body.node.nodes;
    Dec 04 01:31:36 core-01 docker[14455]:                          ^
    Dec 04 01:31:36 core-01 docker[14455]: TypeError: Cannot read property 'node' of undefined
    Dec 04 01:31:36 core-01 docker[14455]:     at Object.<anonymous> (/src/config.js:14:26)
    Dec 04 01:31:36 core-01 docker[14455]:     at Module._compile (module.js:426:26)
    Dec 04 01:31:36 core-01 docker[14455]:     at Object.Module._extensions..js (module.js:444:10)
    Dec 04 01:31:36 core-01 docker[14455]:     at Module.load (module.js:351:32)
    Dec 04 01:31:36 core-01 docker[14455]:     at Function.Module._load (module.js:306:12)
    Dec 04 01:31:36 core-01 docker[14455]:     at Module.require (module.js:361:17)
    Dec 04 01:31:36 core-01 docker[14455]:     at require (module.js:380:17)
    Dec 04 01:31:36 core-01 docker[14455]:     at Object.<anonymous> (/src/app.js:6:14)
    Dec 04 01:31:36 core-01 docker[14455]:     at Module._compile (module.js:426:26)
    Dec 04 01:31:36 core-01 docker[14455]:     at Object.Module._extensions..js (module.js:444:10)
    Dec 04 01:31:36 core-01 docker[14455]:     at Module.load (module.js:351:32)
    Dec 04 01:31:36 core-01 docker[14455]:     at Function.Module._load (module.js:306:12)
    Dec 04 01:31:36 core-01 docker[14455]:     at Function.Module.runMain (module.js:467:10)
    Dec 04 01:31:36 core-01 docker[14455]:     at startup (node.js:117:18)
    Dec 04 01:31:36 core-01 docker[14455]:     at node.js:946:3
    Dec 04 01:31:36 core-01 systemd[1]: todo@1.service: Main process exited, code=exited, status=1/FAILURE
    Dec 04 01:31:37 core-01 systemd[1]: Stopped ToDo Service.
    Dec 04 01:31:37 core-01 systemd[1]: todo@1.service: Unit entered failed state.
    Dec 04 01:31:37 core-01 systemd[1]: todo@1.service: Failed with result 'exit-code'.


<br/>


Проблема возникает из-за неправильного выполнения команды:

    var rethinks = etcd.getSync("/services/rethinkdb", {recursive: true})

<br/>

rethinks получает:

    { err: { [Error: All servers returned error] errors: [ [Object] ], retries: 0 },
      body: undefined,
      headers: undefined }


И далее не может получить список узлов.


    $ sudo su -
    # find / -name app.js
    # vi /var/lib/docker/overlay/bbd29be9551b20e7330c1d65c613e82f089565b9021f8fe80a28677318fc3be5/root/src/config.js


На каждом узле поменять в 1 месте.


    // var nodes = rethinks.body.node.nodes;
    // nodes.forEach(function (node){
    //   console.log('Available rethinkdb server on', node.value);
    // });
    // var rethink = nodes[0].value;

    var rethink = "172.17.8.101";


<br/>



    $ fleetctl stop todo@{1..3} todo-sk@{1..3}
    $ fleetctl start todo@{1..3} todo-sk@{1..3}


<br/>

    $ fleetctl list-units
    UNIT				MACHINE				ACTIVE	SUB
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

    $ etcdctl ls --recursive
    /test
    /test/hello
    /services
    /services/rethinkdb
    /services/rethinkdb/rethinkdb-1
    /services/rethinkdb/rethinkdb-2
    /services/todo
    /services/todo/todo-2
    /services/todo/todo-1
    /services/todo/todo-3
    /coreos.com
    /coreos.com/network
    /coreos.com/network/config
    /coreos.com/network/subnets
    /coreos.com/network/subnets/10.1.99.0-24
    /coreos.com/network/subnets/10.1.34.0-24
    /coreos.com/network/subnets/10.1.29.0-24
    /coreos.com/updateengine
    /coreos.com/updateengine/rebootlock
    /coreos.com/updateengine/rebootlock/semaphore
    /foo
    /foo/bar
    /foo/bar2



<br/>

    $ etcdctl get /services/todo/todo-3
    172.17.8.101:3000


<br/>

    http://172.17.8.101:3000/
    http://172.17.8.102:3000/
    http://172.17.8.103:3000/

<br/>


<div align="center">
    <img src="//files.sysadm.ru/img/linux/containers/coreos/app6.png" border="0" alt="coreos cluster">
</div>    