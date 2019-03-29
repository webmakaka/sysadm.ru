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

В интернетах и в книжных магазинах появилась книга "Kubernetes в действии 2019". Будем изучать и добавлять полезные знания в том числе и из нее.

Kubernets он как квантовая механика. Стоит пол года не касаться и уже почти ничего не помнишь. Надо записывать.

Всякие рассказчики, что kubernetes что-то упрощает, могут смело идти на три буквы.

<br/>

![working with kubernetes](/img/linux/servers/containers/kubernetes/working-with-kubernetes.png "working with kubernetes"){: .center-image }

<br/>

### [Kuberneters на локальном хосте (minikube, cubectl и virtualbox)](/linux/servers/containers/kubernetes/minikube/)

### [Single Master Kubernetes Cluster в виртуалках (vagrant, kubeadm, cubectl)](/linux/servers/containers/kubernetes/kubeadm/)

### [Creating Highly Available Clusters with kubeadm](https://kubernetes.io/docs/setup/independent/high-availability/)

<br/>

## Устарело. Необходима чистка. Удалять пока жалко.

### Первые попытки установить и запустить локально

[Какие-то попытки разобраться с cubectl и minikube](/linux/servers/containers/kubernetes/cubect-minikube/)

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

### Варианты инсталляции kubernetes

https://sysadm.ru/linux/servers/containers/kubernetes/install/

<br/>

### Tutorials

http://kubernetes.io/docs/tutorials/

<br/>

### Kubernetes The Hard Way

https://github.com/kelseyhightower/kubernetes-the-hard-way
