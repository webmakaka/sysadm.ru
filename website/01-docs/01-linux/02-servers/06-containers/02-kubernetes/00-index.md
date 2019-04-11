---
layout: page
title: Kubernetes
permalink: /linux/servers/containers/kubernetes/
---

# Kubernetes

Kubernetes объединяет множество компьютеров в одно целое.

<br/>

k8s = "k" followed by 8 letters followed by "s" (same as i18n = internationalization, l10n = localization)

<br/>

Для изучения обычно достаточно установить minikube на локальном хосте. А потом когда придет понимание, наверное имеет смысл поработать с облачными решениями. Гугл дает \$300 для изучения их технологий один раз и на один год. Сам не пробовал.

Возможно, что собственный локальный кластер на нескольких железках и не понадобится никогда.

В интернетах и в книжных магазинах появилась книга "Kubernetes в действии 2019". Будем изучать и добавлять полезные знания в том числе и из нее.

Kubernets он как квантовая механика. Стоит пол года не касаться и уже почти ничего не помнишь. Надо записывать.

Всякие рассказчики, что kubernetes что-то упрощает, могут смело идти на три буквы.

<br/>

![working with kubernetes](/img/linux/servers/containers/kubernetes/working-with-kubernetes.png "working with kubernetes"){: .center-image }

<br/>

### Толковые (мне нравятся) и актуальные (на 2019) видео по Kubernetes от индуса

<div align="center">
    <iframe width="853" height="480" src="https://www.youtube.com/embed/YzaYqxW0wGs" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>

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

### [Инсталляция клиента для работы с kubernetes кластером (kubectl, minikube)](/linux/servers/containers/kubernetes/install/)

<br/>

### [Основы Kubernets](/linux/servers/containers/kubernetes/basics/)

<br/>

## Более практичные примеры

### [Kuberneters на локальном хосте (minikube, kubectl и virtualbox)](/linux/servers/containers/kubernetes/minikube/)

### [Microk8s (в виртуальных машинах)](/linux/servers/containers/kubernetes/microk8s/)

### [Kuberneters в виртуальных машинах и контейнерах](/linux/servers/containers/kubernetes/kubeadm/)

<br/>

### Материалы для изучения:

**Tutorials**  
http://kubernetes.io/docs/tutorials/

<br/>

**Kubernetes The Hard Way**  
https://github.com/kelseyhightower/kubernetes-the-hard-way
