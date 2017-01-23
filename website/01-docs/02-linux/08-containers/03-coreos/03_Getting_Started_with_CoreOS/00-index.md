---
layout: page
title: Getting Started with CoreOS [29 Nov 2016, ENG]
permalink: /linux/containers/coreos/Getting_Started_with_CoreOS/
---


# [] Getting Started with CoreOS [29 Nov 2016, ENG]


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

Почти!

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
