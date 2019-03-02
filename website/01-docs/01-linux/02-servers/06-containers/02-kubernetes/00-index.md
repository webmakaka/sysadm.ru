---
layout: page
title: Kubernetes
permalink: /linux/servers/containers/kubernetes/
---

# Kubernetes

Kubernetes объединяет множество компьютеров в одно целое.

<br/>

Для изучения обычно достаточно установить minikube на локальном хосте. А потом когда придет понимание, наверное имеет смысл поработать с облачными решениями. Гугл дает \$300 для изучения их технологий один раз и на один год. Сам не пробовал.

Возможно, что собственный локальный кластер на нескольких железках и не понадобится никогда.

В интернетах и в книжных магазинах появилась книга "Kubernetes в действии 2019". Будем изучать и добавлять полезные знания в том числе из нее.

Kubernets он как квантовая механика. Стоит пол года не касаться и уже почти ничего не помнишь. Надо записывать.

Всякие рассказчики, что kubernetes что-то упрощает, могут смело идти на три буквы.

<br/>

![working with kubernetes](/img/linux/servers/containers/kubernetes/working-with-kubernetes.png "working with kubernetes"){: .center-image }

<br/>

### [Kuberneters на локальном хосте (minikube, cubectl и virtualbox)](/linux/servers/containers/kubernetes/minikube/)

<br/>

## Устарело. Необходима чистка. Удалять пока жалко.

### Первые попытки установить и запустить локально

[Какие-то попытки разобраться с cubectl и minikube](/linux/servers/containers/kubernetes/cubect-minikube/)

[Kubernetes - A Multi-Tier Application](/linux/servers/containers/kubernetes/multi-tier-application/)

[Kubernetes > Первое знакомство (устарело)](/linux/servers/containers/kubernetes/first-look/)

[Kubernetes > Устанавливаем локальный кластер (устарело)](/linux/servers/containers/kubernetes/local-cluster/)

<br/>

### На rutracker мне написали:

8-ми узловой кластер прекрасно работает на локальной машине с 16GB RAM!

На Ubuntu ставится одной командой:
conjure-up kubernetes

https://kubernetes.io/docs/getting-started-guides/ubuntu/local/

Если интересует зачем - смотрим сюда:  
Fission: Serverless Functions as a Service for Kubernetes  
https://github.com/fission/fission

<br/>

**Бесплатный курс по kubernetes:**  
https://courses.edx.org/courses/course-v1:LinuxFoundationX+LFS158x+1T2018/course/

**Tutorials**  
http://kubernetes.io/docs/tutorials/

<br/>

**Building blocks:**

-   RCs - Replication Controllers. Заменили на Replica Sets.
-   PODS - минимальная единица управляемая RC. A Pod is a logical collection of one or more containers
-   Services

<br/>

**Kubernets:**

-   One instance for the Master
-   Serveral instances as Minions

<br/>

### Creating a Custom Cluster from Scratch

https://kubernetes.io/docs/getting-started-guides/scratch/

<br/>

### Kubernetes The Hard Way

https://github.com/kelseyhightower/kubernetes-the-hard-way

<br/>

### Варианты инсталляции kubernetes

Kubernetes can be installed using different configurations. The four major installation types are briefly presented below:

-   All-in-One Single-Node Installation
    With all-in-one, all the master and worker components are installed on a single node. This is very useful for learning, development, and testing. This type should not be used in production. Minikube is one such example.

-   Single-Node etcd, Single-Master, and Multi-Worker Installation
    In this setup, we have a single master node, which also runs a single-node etcd instance. Multiple worker nodes are connected to the master node.

-   Single-Node etcd, Multi-Master, and Multi-Worker Installation
    In this setup, we have multiple master nodes, which work in an HA mode, but we have a single-node etcd instance. Multiple worker nodes are connected to the master nodes.

-   Multi-Node etcd, Multi-Master, and Multi-Worker Installation
    In this mode, etcd is configured in a clustered mode, outside the Kubernetes cluster, and the nodes connect to it. The master nodes are all configured in an HA mode, connecting to multiple worker nodes. This is the most advanced and recommended production setup.
