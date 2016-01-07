---
layout: page
title: Vagrant в Linux
permalink: /linux/virtual/vagrant/
---

VirtualBox и Git должны быть установлены.


Инсталляция Vagrant

    # cd /tmp
    # wget https://releases.hashicorp.com/vagrant/1.8.1/vagrant_1.8.1_x86_64.deb

    # dpkg -i vagrant_1.8.1_x86_64.deb

    $ vagrant -v
    Vagrant 1.8.1


<br/>

### Vagrant и CoreOS


https://github.com/coreos/coreos-vagrant/



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


<br/>


    $ vagrant destroy
