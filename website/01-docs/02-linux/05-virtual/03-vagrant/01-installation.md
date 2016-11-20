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
