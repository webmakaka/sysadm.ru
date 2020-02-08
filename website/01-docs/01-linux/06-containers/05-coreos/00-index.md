---
layout: page
title: CoreOS
permalink: /linux/containers/coreos/
---

# CoreOS

<br/>

### Основные сервисы CoreOS

[здесь](/linux/containers/coreos/services/)

<br/>

### Запуск CoreOS с помощью Vargrant

[Запуск CoreOS с помощью Vargrant](/linux/containers/coreos/vagrant-coreos/)

<br/>

### Инсталляция

[VirtualBox и CoreOS](/linux/containers/coreos/install/virtualbox-coreos/)

[Инсталляция CoreOS на 2х виртуальных машинах virtualBox](/linux/containers/coreos/install/virtualbox-coreos-2-machines/)

[Инсталляция CoreOS на хостовую машину](/linux/containers/coreos/install/on-host-machine/)

[Инсталляция Docker Compose на CoreOS](/linux/containers/coreos/install/docker-compose/)

<!-- [Инсталляция Python на CoreOS](https://github.com/sysadm-ru/python-on-coreos/blob/master/install-python-on-coreos.sh) -->

[TOOLBOX](/linux/containers/coreos/toolbox/)

<br/>

### Network

[Настройка сетевых адаптеров в CoreOS](/linux/containers/coreos/network/)

<br/>

### Update | Upgrade

[update](/linux/containers/coreos/update/)

<br/>

### Получить ключик

    $ wget -qO - 'https://discovery.etcd.io/new?size=3'

<!-- /	#	ip	-4	addr	|	grep	inet -->

<br/>

### Примеры запуска


-   Docker dev environment on laptop
-   Small cluster
-   Easy development/testing cluster
-   Production cluster with central services

<br/>

**Small Cluster**

<br/>

[Подготовка окружения для запуска coreos](/linux/containers/coreos/example/env/)

[Пример 1 (Small cluster)](/linux/containers/coreos/example/01/)

[Пример 2 (Small cluster)](/linux/containers/coreos/example/02/)

<br/>

**Easy development/testing cluster**

<br/>


[Пример 3 (Easy development/testing cluster)](/linux/containers/coreos/example/03/)

<br/>

### Using Cloud-Config

https://coreos.com/os/docs/latest/cloud-config.html

<br/>

### cloud-configs validator

https://coreos.com/validate/

<br/>

### Clustering Guide

https://coreos.com/etcd/docs/latest/clustering.html

<br/>

### CoreOS Видеокурсы

[здесь](/linux/containers/coreos/video-courses/)

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
