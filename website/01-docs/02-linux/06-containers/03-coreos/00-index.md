---
layout: page
title: CoreOS
permalink: /linux/containers/coreos/
---


# CoreOS

### Инсталляция

[Vagrant и CoreOS](/linux/containers/coreos/installation/vagrant-coreos/)  

[VirtualBox и CoreOS](/linux/containers/coreos/installation/virtualbox-coreos/)

[Инсталляция CoreOS на 2х виртуальных машинах virtualBox](/linux/containers/coreos/installation/virtualbox-coreos-2-machines/)

[Инсталляция CoreOS на хостовую машину](/linux/containers/coreos/installation/on-host-machine/)

[Инсталляция Docker Compose на CoreOS](/linux/containers/coreos/installation/docker-compose/)


### Network

[Настройка сетевых адаптеров в CoreOS](/linux/containers/coreos/network/)


### Update | Upgrade

[update](/linux/containers/coreos/update/)


<br/><br/>

Etcd — распределенное Key-Value хранилище, которое запускается на каждой машине кластера CoreOS и обеспечивает общий доступ практически ко всем данным в масштабе всего кластера. Внутри etcd хранятся настройки служб, их текущие состояние, конфигурация самого кластера и т.д. Etcd позволяет хранить данные иерархически (хранилище древовидно), подписываться на изменения ключей или целых директорий, задавать для ключей и директорий ключей значения TTL (фактически, «экспирить» их), атомарно изменять или удалять ключи, упорядоченно хранить их (что позволяет реализовывать своеобразные очереди). Поскольку конфигурация сервисов, запущенных в масштабе кластера, хранится в etcd, узнать о запуске и остановке того или иного сервиса можно просто подписавшись на изменения соответствующих ключей в хранилище.

Etcd - похоже на Consul и ZooKeeper. (Лично я ничего из этого пока не знаю).

    etcdctl set /message Hello
    etcdctl get /message
    etcdctl mkdir /foo-service
    etcdctl set /foo-service/container1 localhost:1111
    etcdctl ls /foo-service
    etcdctl set /foo "Expiring Soon" --ttl 20
    etcdctl watch /foo-service --recursive


<br/>

<div align="center">
    <img src="//files.sysadm.ru/img/linux/containers/coreos/etcd.png" border="0" alt="etcd">
</div>

<br/>



Fleet — (коротко и упрощенно - distributed systemd) это «надстройка» над systemd, которая переносит управление службами с локальной машины на уровень кластера. Fleet хранит конфигурацию служб в виде юнитов systemd (в etcd), автоматически доставляет ее на локальные машины, запускает, перезапускает (при необходимости), останавливает службы на машинах кластера. Fleet умеет планировать запуск служб исходя из загруженности конкретных машин кластера. Ему можно сказать, что конкретную службу нужно запускать только на определенных машинах и т.д.

    fleetctl list-machines
    fleetctl start redis.service
    fleetctl journal redis.service
    fleetctl --tunnel=10.2.1.1 list-machines



Kubernetes - более продвинутый аналог fleet  

flannel is a virtual network that gives a subnet to each host for use with container runtimes.

<br/><br/>


### [O’Reilly Media / Infinite Skills] Introduction to CoreOS Training Video [2015, ENG]

<ul>
    <li><a href="/linux/containers/coreos/Introduction_to_CoreOS/">[O’Reilly Media / Infinite Skills] Introduction to CoreOS Training Video [2015, ENG]</a></li>
</ul>


<br/>

Еще неплохое видео:

http://www.youtube.com/watch?v=wxUxtflalE4

https://github.com/sysadm-ru/production-docker-ha-architecture




<br/><br/>

**Возможно, полезные статьи по CoreOS:**


<ul>


    <li><a href="https://www.digitalocean.com/community/tutorials/how-to-use-fleet-and-fleetctl-to-manage-your-coreos-cluster" rel="nofollow">How To Use Fleet and Fleetctl to Manage your CoreOS Cluster</a></li>

    <li><a href="https://habrahabr.ru/post/244585/" rel="nofollow">CoreOS — Linux для минималистичных кластеров. Коротко</a></li>

    <li><a href="https://www.digitalocean.com/community/tutorial_series/getting-started-with-coreos-2" rel="nofollow">Getting Started with CoreOS</a></li>

    <li><a href="http://www.currah.ca/tech/2015/10/08/kubernetes-coreos.html" rel="nofollow">CoreOS w/ Kubernetes Install Guide for VirtualBox</a></li>

</ul>
