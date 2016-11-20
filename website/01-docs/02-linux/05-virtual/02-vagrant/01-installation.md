---
layout: page
title: Инсталляция Vargant в Ubuntu 14.04
permalink: /linux/virtual/vagrant/installation/ubuntu-14-04/
---


# Инсталляция Vargant в Ubuntu 14.04

Пока docker выглядит для меня намного предпочтительней. Правда как оказалось, с помощью vagrant можно создавать множество docker контейнеров.


<br/>

### Инсталляция Vagrant

    # cd /tmp
    # wget https://releases.hashicorp.com/vagrant/1.8.1/vagrant_1.8.1_x86_64.deb

    # dpkg -i vagrant_1.8.1_x86_64.deb

    $ vagrant -v
    Vagrant 1.8.1


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


Рекомендуется к просмотру:  

[tutsplus.com] Easy Rails Development Environment With Vagrant [2015, ENG]
