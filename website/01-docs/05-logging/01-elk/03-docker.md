---
layout: page
title: Запуск ELK стека в Docker контейнере
description: Запуск ELK стека в Docker контейнере
keywords: linux, logging, elk, docker
permalink: /linux/logging/elk/docker/
---

# Запуск ELK стека в Docker контейнере

<br/>

Делаю  
13.05.2019

По материалам индуса:

https://www.youtube.com/watch?v=U8Iq4vm2Ekk&list=PL34sAs7_26wOgpqMW_0_E95k9tq2VkMOZ&index=2

<br/>
Рисунок индуса:
<br/>

![elk docker](/img/adm/logging/elk/intall/elk-docker.png 'elk docker'){: .center-image }

<br/>

Для прода рекомендуется ознакомиться с материалом:  
https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html

<br/>

    $ mkdir ~/tmp/elk && cd ~/tmp/elk

    # git clone https://bitbucket.org/sysadm-ru/vagrant.git

    $ cd vagrant/vagrantfiles/ubuntu18/

    $ vagrant up

    $ vagrant ssh

<br/>

Инсталлируем <a href="//gitops.ru/devops/containers/docker/setup/ubuntu/">docker и docker-compose</a>.

<br/>

    # vi /etc/sysctl.conf

Добавляем:

```
#################
# User defined
#
vm.max_map_count=262144
```

<br/>

    // Применить параметры до перезагрузки
    # sysctl -w vm.max_map_count=262144

    // Убедиться что параметр установлен правильно
    # sysctl -a | grep vm.max_map_count

<br/>

    # su - vagrant
    $ mkdir ~/tmp && cd ~/tmp
    $ git clone https://bitbucket.org/sysadm-ru/elk.git

    $ cd ~/tmp/elk/docker
    $ docker-compose up

<br/>

http://192.168.0.11:5601

<br/>

### Удалить созданную виртуальную машину (когда перестанет быть нужной)

    $ docker-compose rm -f
