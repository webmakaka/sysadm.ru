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

    $ journalctl --unit etcd.service --no-pager

<br/>


    core@my_vm01 ~ $ fleetctl list-machines
    Error retrieving list of active machines: googleapi: Error 503: fleet server unable to communicate with etcd

<br/>

    core@my_vm01 ~ $ fleetctl list-units
    Error retrieving list of units from repository: googleapi: Error 503: fleet server unable to communicate with etcd
