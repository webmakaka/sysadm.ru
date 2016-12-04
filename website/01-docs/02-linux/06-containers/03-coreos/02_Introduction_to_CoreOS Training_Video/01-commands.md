---
layout: page
title: CoreOS команды
permalink: /linux/containers/coreos/commands/
---


### CoreOS команды

<br/>

    $ ss -lnt
    State      Recv-Q Send-Q Local Address:Port               Peer Address:Port              
    LISTEN     0      128          *:5355                     *:*                  
    LISTEN     0      128         :::5355                    :::*                  
    LISTEN     0      128         :::22                      :::*                  


<br/>

### journalctl

    $ journalctl --unit etcd.service --no-pager
