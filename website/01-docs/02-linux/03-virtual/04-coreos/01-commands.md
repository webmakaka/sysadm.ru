---
layout: page
title: CoreOS команды
permalink: /linux/virtual/coreos/commands/
---



    $ systemctl status docker
    ● docker.service - Docker Application Container Engine
       Loaded: loaded (/usr/lib64/systemd/system/docker.service; disabled; vendor preset: disabled)
       Active: active (running) since Sat 2016-01-16 15:52:01 UTC; 20h ago
         Docs: http://docs.docker.com
     Main PID: 6889 (docker)
       Memory: 695.2M
          CPU: 3min 55.607s
       CGroup: /system.slice/docker.service
               └─6889 docker daemon --host=fd://

    Jan 16 15:58:16 my_vm01 dockerd[6889]: time="2016-01-16T15:58:16.126897656Z...n"
    Jan 16 16:00:08 my_vm01 dockerd[6889]: time="2016-01-16T16:00:08.206281177Z...n"
    Jan 16 16:00:08 my_vm01 dockerd[6889]: time="2016-01-16T16:00:08.250226059Z...b"
    Jan 16 16:00:39 my_vm01 dockerd[6889]: time="2016-01-16T16:00:39.599895114Z...a"
    Jan 16 16:03:01 my_vm01 dockerd[6889]: time="2016-01-16T16:03:01.993524980Z...1"
    Jan 16 16:06:51 my_vm01 dockerd[6889]: time="2016-01-16T16:06:51.597098575Z...1"
    Jan 16 16:06:54 my_vm01 dockerd[6889]: time="2016-01-16T16:06:54.121200420Z...n"
    Jan 16 16:24:17 my_vm01 dockerd[6889]: time="2016-01-16T16:24:17.123640907Z...n"
    Jan 16 16:24:17 my_vm01 dockerd[6889]: time="2016-01-16T16:24:17.150657359Z...c"
    Jan 17 12:06:20 my_vm01 dockerd[6889]: time="2016-01-17T12:06:20.555830563Z...n"
    Hint: Some lines were ellipsized, use -l to show in full.

<br/>


$ sudo systemctl status etcd
● etcd.service - etcd
   Loaded: loaded (/usr/lib64/systemd/system/etcd.service; static; vendor preset: disabled)
   Active: active (running) since Sun 2016-01-17 12:37:31 UTC; 11s ago
 Main PID: 31689 (etcd)
   Memory: 9.2M
      CPU: 47ms
   CGroup: /system.slice/etcd.service
           └─31689 /usr/bin/etcd

Jan 17 12:37:31 my_vm01 systemd[1]: Started etcd.
Jan 17 12:37:32 my_vm01 etcd[31689]: [etcd] Jan 17 12:37:32.455 INFO      |...er
Jan 17 12:37:32 my_vm01 etcd[31689]: [etcd] Jan 17 12:37:32.462 INFO      |...1]
Jan 17 12:37:32 my_vm01 etcd[31689]: [etcd] Jan 17 12:37:32.463 INFO      |...1]
Jan 17 12:37:32 my_vm01 etcd[31689]: [etcd] Jan 17 12:37:32.463 INFO      |...de
Jan 17 12:37:32 my_vm01 etcd[31689]: [etcd] Jan 17 12:37:32.464 INFO      |...'.
Jan 17 12:37:32 my_vm01 etcd[31689]: [etcd] Jan 17 12:37:32.465 INFO      |...'.
Jan 17 12:37:32 my_vm01 etcd[31689]: [etcd] Jan 17 12:37:32.465 INFO      |...'.
Hint: Some lines were ellipsized, use -l to show in full.


<br/>

    $ ss -lnt
    State      Recv-Q Send-Q Local Address:Port               Peer Address:Port              
    LISTEN     0      128          *:5355                     *:*                  
    LISTEN     0      128         :::5355                    :::*                  
    LISTEN     0      128         :::22                      :::*                  


<br/>

    $ systemctl status etcd2
    ● etcd2.service - etcd2
       Loaded: loaded (/usr/lib64/systemd/system/etcd2.service; disabled; vendor preset: disabled)
      Drop-In: /run/systemd/system/etcd2.service.d
               └─20-cloudinit.conf
       Active: activating (auto-restart) (Result: exit-code) since Sun 2016-01-17 12:21:27 UTC; 6s ago
      Process: 30921 ExecStart=/usr/bin/etcd2 (code=exited, status=1/FAILURE)
     Main PID: 30921 (code=exited, status=1/FAILURE)

    Jan 17 12:21:27 my_vm01 systemd[1]: etcd2.service: Main process exited, cod...RE
    Jan 17 12:21:27 my_vm01 systemd[1]: Failed to start etcd2.
    Jan 17 12:21:27 my_vm01 systemd[1]: etcd2.service: Unit entered failed state.
    Jan 17 12:21:27 my_vm01 systemd[1]: etcd2.service: Failed with result 'exit...'.
    Hint: Some lines were ellipsized, use -l to show in full.







<br/>

### etcdctl

    $ etcdctl ls /coreos.com
    Error:  client: etcd cluster is unavailable or misconfigured
    error #0: dial tcp 127.0.0.1:4001: connection refused
    error #1: dial tcp 127.0.0.1:2379: connection refused


<br/>

### fleetctl

    $ fleetctl list-units

    $ fleetctl list-machines

    $ fleetctl list-unit-files

    $ fleetctl unload todo@.service

    $ fleetctl submit todo*

    $ fleetctl start todo@{1..3}

    $ fleetctl destroy todo-sk@1.service

    $ fleetctl journal -f --lines=50 rethinkdb@1

<br/>

### journalctl

    $ journalctl --unit etcd.service --no-pager
