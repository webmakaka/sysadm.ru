---
layout: page
title: Getting Started with CoreOS [29 Nov 2016, ENG]
permalink: /linux/containers/coreos/Getting_Started_with_CoreOS/
---


# Getting Started with CoreOS [29 Nov 2016, ENG]


https://github.com/sysadm-ru/CoreOS-GettingStarted


<br/>

## Deploying and Running Containers


Стартуем coreos также как и в предыдущем курсе.
Правда конфиг в этот раз не меняли вовсе.


    $ vagrant up
    $ vagrant ssh core-01 -- -A

<br/>

    $ sudo su -

    # vi /etc/systemd/system/hello.service

Почти из этого файла!

https://github.com/sysadm-ru/CoreOS-GettingStarted/blob/master/RunningAndConfiguring/hello.service

<br/>


    [Unit]
    Description=MyApp
    After=docker.service
    Requires=docker.service

    [Service]
    TimeoutStartSec=0
    ExecStartPre=-/usr/bin/docker kill %p
    ExecStartPre=-/usr/bin/docker rm %p
    ExecStartPre=/usr/bin/docker pull busybox
    ExecStart=/usr/bin/docker run --name %p busybox /bin/sh -c "while true; do echo Hello World; sleep 1; done"
    ExecStop=/usr/bin/docker stop %p

    [Install]
    WantedBy=multi-user.target


<br/>

    # systemctl enable /etc/systemd/system/hello.service

    # systemctl start /etc/systemd/system/hello.service

    # systemctl list-units

    # systemctl list-units hello.service

    # systemctl status hello.service

    # journalctl -u hello.service

    # journalctl -f -u hello.service

<br/>

    # systemctl daemon-reload

    # systemctl restart /etc/systemd/system/hello.service

<br/>

### etcd


<br/>

<div align="center">
    <img src="//files.sysadm.ru/img/linux/containers/coreos/getting_started_with_coreos/pic1.png" border="0" alt="cluster">
</div>

<br/>

<div align="center">
    <img src="//files.sysadm.ru/img/linux/containers/coreos/getting_started_with_coreos/pic2.png" border="0" alt="cluster">
</div>

<br/>


Стартовали vagrant со следующим конфигом.

только 3 машины + discovery

    https://github.com/sysadm-ru/CoreOS-GettingStarted/blob/master/RunningAndConfiguring/user-data.sample

<br/>

    $ etcd cluster-health



<br/>

### Fleet



    $ vagrant status
    Current machine states:

    core-01                   running (virtualbox)
    core-02                   running (virtualbox)
    core-03                   running (virtualbox)

<br/>

    $ vagrant ssh-config
    Host core-01
      HostName 127.0.0.1
      User core
      Port 2222
      UserKnownHostsFile /dev/null
      StrictHostKeyChecking no
      PasswordAuthentication no
      IdentityFile /home/marley/.vagrant.d/insecure_private_key
      IdentitiesOnly yes
      LogLevel FATAL
      ForwardAgent yes

    Host core-02
      HostName 127.0.0.1
      User core
      Port 2203
      UserKnownHostsFile /dev/null
      StrictHostKeyChecking no
      PasswordAuthentication no
      IdentityFile /home/marley/.vagrant.d/insecure_private_key
      IdentitiesOnly yes
      LogLevel FATAL
      ForwardAgent yes

    Host core-03
      HostName 127.0.0.1
      User core
      Port 2207
      UserKnownHostsFile /dev/null
      StrictHostKeyChecking no
      PasswordAuthentication no
      IdentityFile /home/marley/.vagrant.d/insecure_private_key
      IdentitiesOnly yes
      LogLevel FATAL
      ForwardAgent yes


<br/>

      $ ssh-add ~/.vagrant.d/insecure_private_key
      Identity added: /home/marley/.vagrant.d/insecure_private_key (/home/marley/.vagrant.d/insecure_private_key)


      $ ssh -p 2222 -i ~/.vagrant.d/insecure_private_key


<br/>

    $ ssh core-01

<br/>


    $ fleetctl list-machines
    MACHINE		IP		METADATA
    20fd3ecb...	172.17.8.102	-
    89e20c71...	172.17.8.103	-
    d8ed170f...	172.17.8.101	-

<br/>

    $ fleetctl list-units   
    UNIT	MACHINE	ACTIVE	SUB


    $ ssh-add ~/.vagrant.d/insecure_private_key

    $ fleetctl --tunnel 127.0.0.1:2222 list-machines


    $ FLEETCTL_TUNNEL="127.0.0.1:2222"

    $ fleetctl list-machines
