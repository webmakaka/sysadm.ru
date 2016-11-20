---
layout: page
title: Инсталляция CoreOS с помощью Vargrant
permalink: /linux/containers/coreos/installation/vagrant-coreos/
---


### Инсталляция CoreOS с помощью Vargrant

VirtualBox и Git должны быть установлены.


### Vagrant и CoreOS

    $ git clone https://github.com/coreos/coreos-vagrant/

    $ cd coreos-vagrant/

    $ mv config.rb.sample config.rb
    $ mv user-data.sample user-data


<br/>

    $ vi config.rb

    $num_instances=3

    $update_channel='stable'

<br/>

Генерируем  
https://discovery.etcd.io/new

<br/>

    $ vi user-data

    discovery: https://discovery.etcd.io/29fb47f99d2b8ee4d10d0ce49f350a0f


<br/>

    $ vagrant up

<br/>

    $ vagrant status
    Current machine states:

    core-01                   running (virtualbox)
    core-02                   running (virtualbox)
    core-03                   running (virtualbox)


<br/>

    $ vagrant ssh core-01 -- -A

<br/>

    $ core@core-01 ~ $ fleetctl list-machines
    MACHINE		IP		METADATA
    48777333...	172.17.8.103	-
    5127495f...	172.17.8.102	-
    e20b10bc...	172.17.8.101	-


<br/>

    // Если будет нужно выключить
    $ vagrant destroy
