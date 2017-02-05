---
layout: page
title: CoreOS
permalink: /linux/containers/coreos/
---


# CoreOS

<br/>

### Инсталляция

[Vagrant и CoreOS](/linux/containers/coreos/installation/vagrant-coreos/)  

[VirtualBox и CoreOS](/linux/containers/coreos/installation/virtualbox-coreos/)

[Инсталляция CoreOS на 2х виртуальных машинах virtualBox](/linux/containers/coreos/installation/virtualbox-coreos-2-machines/)

[Инсталляция CoreOS на хостовую машину](/linux/containers/coreos/installation/on-host-machine/)

[Инсталляция Docker Compose на CoreOS](/linux/containers/coreos/installation/docker-compose/)


[Инсталляция Python на CoreOS](https://github.com/sysadm-ru/python-on-coreos/blob/master/install-python-on-coreos.sh)

[TOOLBOX](/linux/containers/coreos/toolbox/)

<br/>

### Network

[Настройка сетевых адаптеров в CoreOS](/linux/containers/coreos/network/)


<br/>

### Update | Upgrade

[update](/linux/containers/coreos/update/)


<br/>

### Using Cloud-Config

https://coreos.com/os/docs/latest/cloud-config.html


<br/>

### Clustering Guide

https://coreos.com/etcd/docs/latest/clustering.html


<br/>

### Терминология

Etcd — распределенное Key-Value хранилище, которое запускается на каждой машине кластера CoreOS и обеспечивает общий доступ практически ко всем данным в масштабе всего кластера. Внутри etcd хранятся настройки служб, их текущие состояние, конфигурация самого кластера и т.д. Etcd позволяет хранить данные иерархически (хранилище древовидно), подписываться на изменения ключей или целых директорий, задавать для ключей и директорий ключей значения TTL (фактически, «экспирить» их), атомарно изменять или удалять ключи, упорядоченно хранить их (что позволяет реализовывать своеобразные очереди). Поскольку конфигурация сервисов, запущенных в масштабе кластера, хранится в etcd, узнать о запуске и остановке того или иного сервиса можно просто подписавшись на изменения соответствующих ключей в хранилище.

Etcd - похоже на Consul и ZooKeeper. (Лично я ничего из этого пока не знаю).

    $ etcdctl set /message Hello
    $ etcdctl get /message
    $ etcdctl mkdir /foo-service
    $ etcdctl set /foo-service/container1 localhost:1111
    $ etcdctl ls /foo-service
    $ etcdctl set /foo "Expiring Soon" --ttl 20
    $ etcdctl watch /foo-service --recursive

    $ etcdctl cluster-health
    member 29dac19a68d1b860 is healthy: got healthy result from http://172.17.8.101:2379
    member b5e1282b428f7211 is healthy: got healthy result from http://172.17.8.103:2379
    member ecd0a5d052505a2f is healthy: got healthy result from http://172.17.8.102:2379
    cluster is healthy



<br/>

<div align="center">
    <img src="//files.sysadm.ru/img/linux/containers/coreos/etcd.png" border="0" alt="etcd">
</div>

<br/>

<br/>

<div align="center">
    <img src="//files.sysadm.ru/img/linux/containers/coreos/getting_started_with_coreos/pic1.png" border="0" alt="coreos cluster">
</div>

<br/>

<div align="center">
    <img src="//files.sysadm.ru/img/linux/containers/coreos/getting_started_with_coreos/pic2.png" border="0" alt="coreos cluster">
</div>

<br/>

**Аналоги:**

- consul
- ZooKeeper


<br/>

### Fleet

Fleet — (коротко и упрощенно - distributed systemd) это «надстройка» над systemd, которая переносит управление службами с локальной машины на уровень кластера. Fleet хранит конфигурацию служб в виде юнитов systemd (в etcd), автоматически доставляет ее на локальные машины, запускает, перезапускает (при необходимости), останавливает службы на машинах кластера. Fleet умеет планировать запуск служб исходя из загруженности конкретных машин кластера. Ему можно сказать, что конкретную службу нужно запускать только на определенных машинах и т.д.



<br/>

<div align="center">
    <img src="//files.sysadm.ru/img/linux/containers/coreos/getting_started_with_coreos/pic3.png" border="0" alt="fleetctl">
</div>

<br/>


    fleetctl list-machines
    fleetctl start redis.service
    fleetctl journal redis.service
    fleetctl --tunnel=10.2.1.1 list-machines


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


<br/>

Kubernetes - более продвинутый аналог fleet  


### Flannel

flannel - виртуальная сеть, которая предоставляет подсеть, чтобы контейнеры могли между собой обмениваться пакетами. (я так перевел / понял)


<br/>

### journalctl

    # journalctl -u hello.service
    # journalctl -f -u hello.service

<br/>


### [O’Reilly Media / Infinite Skills] Introduction to CoreOS Training Video [2015, ENG]

<ul>
    <li><a href="/linux/containers/coreos/Introduction_to_CoreOS/">[O’Reilly Media / Infinite Skills] Introduction to CoreOS Training Video [2015, ENG]</a></li>
</ul>


<br/>


### Getting Started with CoreOS [29 Nov 2016, ENG]

<ul>
    <li><a href="/linux/containers/coreos/Getting_Started_with_CoreOS/">Getting Started with CoreOS [29 Nov 2016, ENG]</a></li>
</ul>


<br/>

### Еще неплохое видео по coreos:

http://www.youtube.com/watch?v=wxUxtflalE4

https://github.com/sysadm-ru/production-docker-ha-architecture





<br/>
<br/>

### Команды

[Команды (пока разбираемся)](/linux/containers/coreos/commands/)  
[cloud-config (пока разбираемся)](/linux/containers/coreos/cloud-config/)


confd:  
https://github.com/kelseyhightower/confd


<br/><br/>

**Возможно, полезные статьи по CoreOS:**


<ul>


    <li><a href="https://www.digitalocean.com/community/tutorials/how-to-use-fleet-and-fleetctl-to-manage-your-coreos-cluster" rel="nofollow">How To Use Fleet and Fleetctl to Manage your CoreOS Cluster</a></li>

    <li><a href="https://habrahabr.ru/post/244585/" rel="nofollow">CoreOS — Linux для минималистичных кластеров. Коротко</a></li>

    <li><a href="https://www.digitalocean.com/community/tutorial_series/getting-started-with-coreos-2" rel="nofollow">Getting Started with CoreOS</a></li>

    <li><a href="http://www.currah.ca/tech/2015/10/08/kubernetes-coreos.html" rel="nofollow">CoreOS w/ Kubernetes Install Guide for VirtualBox</a></li>

</ul>
