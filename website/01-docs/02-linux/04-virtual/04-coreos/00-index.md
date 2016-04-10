---
layout: page
title: CoreOS
permalink: /linux/virtual/coreos/
---


### Инсталляция

[Vagrant и CoreOS](/linux/virtual/coreos/installation/vagrant-coreos/)  

[VirtualBox и CoreOS](/linux/virtual/coreos/installation/virtualbox-coreos/)

[Инсталляция CoreOS на 2х виртуальных машинах virtualBox](/linux/virtual/coreos/installation/virtualbox-coreos-2-machines/)

[Инсталляция CoreOS на хостовую машину](/linux/virtual/coreos/installation/on-host-machine/)

[Инсталляция Docker Compose на CoreOS](/linux/virtual/coreos/installation/docker-compose/)

<br/>

### Network

[Настройка сетевых адаптеров в CoreOS](/linux/virtual/coreos/network/)


<br/><br/>

### Команды

[Команды (пока разбираемся)](/linux/virtual/coreos/commands/)  
[cloud-config (пока разбираемся)](/linux/virtual/coreos/cloud-config/)

<br/><br/>

Etcd — распределенное Key-Value хранилище, которое запускается на каждой машине кластера CoreOS и обеспечивает общий доступ практически ко всем данным в масштабе всего кластера. Внутри etcd хранятся настройки служб, их текущие состояние, конфигурация самого кластера и т.д. Etcd позволяет хранить данные иерархически (хранилище древовидно), подписываться на изменения ключей или целых директорий, задавать для ключей и директорий ключей значения TTL (фактически, «экспирить» их), атомарно изменять или удалять ключи, упорядоченно хранить их (что позволяет реализовывать своеобразные очереди). Поскольку конфигурация сервисов, запущенных в масштабе кластера, хранится в etcd, узнать о запуске и остановке того или иного сервиса можно просто подписавшись на изменения соответствующих ключей в хранилище.


Fleet — это «надстройка» над systemd, которая переносит управление службами с локальной машины на уровень кластера. Fleet хранит конфигурацию служб в виде юнитов systemd (в etcd), автоматически доставляет ее на локальные машины, запускает, перезапускает (при необходимости), останавливает службы на машинах кластера. Fleet умеет планировать запуск служб исходя из загруженности конкретных машин кластера. Ему можно сказать, что конкретную службу нужно запускать только на определенных машинах и т.д.



Kubernetes - более продвинутый аналог fleet  

flannel is a virtual network that gives a subnet to each host for use with container runtimes.

<br/><br/>


Видео:

http://www.youtube.com/watch?v=wxUxtflalE4

https://github.com/sysadm-ru/production-docker-ha-architecture


<br/><br/>

Смотрю видео курс
[O'Reilly Media / Infinite Skills] Introduction to CoreOS Training Video [2015, ENG]


https://coreos.com/os/docs/latest/booting-on-vagrant.html

https://github.com/coreos/coreos-vagrant/

https://github.com/rosskukulinski/Introduction_To_CoreOS/







[Приступаю к изучению](/linux/virtual/coreos/service-example/)  

[DataBase Layer](/linux/virtual/coreos/coreos-database-layer/)  


[Application Layer](/linux/virtual/coreos/coreos-application-layer/)

[Load balancing with NGINX](/linux/virtual/coreos/load-balancing-with-nginx/)

[Load balancing with HAPROXY & CONFD](/linux/virtual/coreos/load-balancing-with-haproxy-and-confd/)





<br/><br/>

**Возможно, полезные статьи по docker:**


<ul>

<li><a href="https://habrahabr.ru/post/244585/" rel="nofollow">CoreOS — Linux для минималистичных кластеров. Коротко</a></li>

<li><a href="https://www.digitalocean.com/community/tutorial_series/getting-started-with-coreos-2" rel="nofollow">Getting Started with CoreOS</a></li>

<li><a href="http://www.currah.ca/tech/2015/10/08/kubernetes-coreos.html" rel="nofollow">CoreOS w/ Kubernetes Install Guide for VirtualBox</a></li>

</ul>
