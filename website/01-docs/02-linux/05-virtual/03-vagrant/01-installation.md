---
layout: page
title: Инсталляция Vargant в Ubuntu 14.04
permalink: /linux/virtual/vagrant/installation/ubuntu-14-04/
---


# Инсталляция Vargant в Ubuntu 14.04


DOWNLOAD VAGRANT:  
https://www.vagrantup.com/downloads.html

<br/>

### Инсталляция Vagrant

    # cd /tmp
    # wget https://releases.hashicorp.com/vagrant/1.8.7/vagrant_1.8.7_x86_64.deb

    # dpkg -i vagrant_1.8.7_x86_64.deb

    $ vagrant -v
    Vagrant 1.8.7


<br/>

### Создание виртуальной машины с помощью Vagrant

Подготовленные виртуальные машины (можно бесплатно завести свою учетку):  
https://atlas.hashicorp.com/boxes/search


    // Инициализировать конфиг файл с определенной операционной системой
    $ vagrant init bento/ubuntu-14.04


    // Задать параметры создаваемой виртуальной машины
    $ vi Vagrantfile

    // Запустить
    $ vagrant up

    // Подключиться
    $ vagrant ssh

    // остановить
    $ vagrant halt

    // перезагрузить имиджи (если я все правильно понял)
    $ vagrant reload

    //
    $ vagrant destroy



<br/>


    $ vagrant ssh core-01 -- -A
    A Vagrant environment or target machine is required to run this
    command. Run `vagrant init` to create a new Vagrant environment. Or,
    get an ID of a target machine from `vagrant global-status` to run
    this command on. A final option is to change to a directory with a
    Vagrantfile and to try again.


    marley@workstation:/projects/sysadm.ru$ vagrant global-status
    id       name    provider   state   directory                           
    ------------------------------------------------------------------------
    d2681b6  core-01 virtualbox running /home/marley/coreos-vagrant         
    bf4323b  core-02 virtualbox running /home/marley/coreos-vagrant         
    1151104  core-03 virtualbox running /home/marley/coreos-vagrant         

    The above shows information about all known Vagrant environments
    on this machine. This data is cached and may not be completely
    up-to-date. To interact with any of the machines, you can go to
    that directory and run Vagrant, or you can use the ID directly
    with Vagrant commands from any directory. For example:
    "vagrant destroy 1a2b3c4d"


    $ vagrant ssh d2681b6
    Last login: Sat Dec  3 22:16:56 UTC 2016 from 10.0.2.2 on pts/0
    CoreOS stable (1185.3.0)
    Failed Units: 3
      helloworld.service
      todo-sk@1.service
      todo@1.service
