---
layout: page
title: Vagrant и CoreOS
permalink: /linux/virtual/vagrant/coreos-clusters/
---


### Vagrant и CoreOS

    $ git clone https://github.com/coreos/coreos-vagrant/

    $ cd coreos-vagrant/

    $ mv config.rb.sample config.rb
    $ mv user-data.sample user-data

    $ atom .

<br/>

    config.rb

    # $num_instances=1
    $num_instances=3

    #$update_channel='alpha'
    $update_channel='stable'


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


Возможно, что нужно будет сгенерировать ключ и вставить его в user-data.

https://discovery.etcd.io/new

discovery: https://discovery.etcd.io/29fb47f99d2b8ee4d10d0ce49f350a0f

<br/>

    // Если будет нужно выключить
    $ vagrant destroy
