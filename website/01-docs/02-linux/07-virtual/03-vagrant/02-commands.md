---
layout: page
title: Команды Vagrant
permalink: /linux/virtual/vagrant/commands/
---


# Команды Vagrant


<br/>

### Создание виртуальной машины с помощью Vagrant

Подготовленные виртуальные машины:  
https://app.vagrantup.com/boxes/search


<br/>

Vagrant копирует виртуальные машины с следующие директории. 

<br/>

    $ pwd
    /home/marley/.vagrant.d/boxes

    $ ls
    bento-VAGRANTSLASH-ubuntu-14.04  hashicorp-VAGRANTSLASH-precise64
    coreos-alpha                     ubuntu-VAGRANTSLASH-bionic64
    coreos-beta                      ubuntu-VAGRANTSLASH-trusty64
    coreos-stable                    ubuntu-VAGRANTSLASH-xenial64
    debian-VAGRANTSLASH-jessie64     v0rtex-VAGRANTSLASH-xenial64


<br/>


Если версия обновляется, то в каталогах присутствуют разные версии. Может занимать много места.


    $ cd ubuntu-VAGRANTSLASH-trusty64
    
    $ ls
    20161214.0.0  20170208.0.0  metadata_url

<br/>

### Команды

    // Инициализировать конфиг файл с определенной операционной системой
    $ vagrant init ubuntu/bionic64


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

    // Destroy without confirmation
    $ vagrant destroy -f

<br/>

    $ vagrant box list
    coreos-alpha        (virtualbox, 1298.1.0)
    coreos-stable       (virtualbox, 1185.3.0)
    coreos-stable       (virtualbox, 1235.6.0)
    coreos-stable       (virtualbox, 1235.9.0)
    debian/jessie64     (virtualbox, 8.7.0)
    hashicorp/precise64 (virtualbox, 1.1.0)
    ubuntu/trusty64     (virtualbox, 20161214.0.0)
    ubuntu/trusty64     (virtualbox, 20170208.0.0)
    ubuntu/xenial64     (virtualbox, 20161213.0.0)
    ubuntu/xenial64     (virtualbox, 20170209.0.0)


<br/>

### Ошибка


    $ vagrant ssh core-01 -- -A
    A Vagrant environment or target machine is required to run this
    command. Run `vagrant init` to create a new Vagrant environment. Or,
    get an ID of a target machine from `vagrant global-status` to run
    this command on. A final option is to change to a directory with a
    Vagrantfile and to try again.


<br/>

    $ vagrant global-status
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

<br/>

    $ vagrant ssh d2681b6
    Last login: Sat Dec  3 22:16:56 UTC 2016 from 10.0.2.2 on pts/0
    CoreOS stable (1185.3.0)
    Failed Units: 3
      helloworld.service
      todo-sk@1.service
      todo@1.service



<br/>      

### Еще команды:

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


  // Чтобы можно было по ssh ходить между узлами без пароля

    $ ssh-add ~/.vagrant.d/insecure_private_key
    Identity added: /home/marley/.vagrant.d/insecure_private_key (/home/marley/.vagrant.d/insecure_private_key)

Потом:

    $ ssh core-01 -p 2222 -i ~/.vagrant.d/insecure_private_key


<br/>

      $ ssh core-01

<!--

<br/>

    $ ssh-keygen

<br/>

    $ ls ~/.ssh/
    id_rsa  id_rsa.pub



$ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

$ cat ~/.ssh/id_rsa >> ~/.ssh/insecure_private_key


-->
