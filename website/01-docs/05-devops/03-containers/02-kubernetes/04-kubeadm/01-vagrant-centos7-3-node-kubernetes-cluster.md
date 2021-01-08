---
layout: page
title: Скрипты, разворачивающие Single Master Kubernetes Cluster в VirtualBox
description: Скрипты, разворачивающие Single Master Kubernetes Cluster в VirtualBox
keywords: devops, kubernetes, virtualbox, centos7
permalink: /devops/containers/kubernetes/kubeadm/vagrant-centos7-3-node-kubernetes-cluster/
---

# Скрипты, разворачивающие Single Master Kubernetes Cluster в VirtualBox

Делаю  
10.04.2020

Предполагается что уже установлен <a href="/linux/virtual/virtualbox/install/">VirtualBox</a>, <a href="/linux/virtual/vagrant/install/ubuntu/">Vagrant</a>, <a href="/devops/containers/kubernetes/install/">kubectl</a>.

Разворачиваются 3 виртуалки по 2GB оперативной памяти. По идее, на узлы вполне достаточно и 1GB.

<br/>

### На хост машине

    $ sudo vi /etc/hosts

```
#---------------------------------------------------------------------
# Kubernetes cluster
#---------------------------------------------------------------------

192.168.0.10 master.k8s master
192.168.0.11 node1.k8s node1
192.168.0.12 node2.k8s node2
```

<br/>

    $ vagrant plugin install vagrant-hostmanager

<br/>

    $ mkdir ~/vagrant-kubernetes && cd ~/vagrant-kubernetes

    $ git clone https://github.com/webmakaka/vagrant-kubernetes-3-node-cluster-centos7 .

    // Скрипты для установки актуальной версии kubernetes сluster
    $ cd ~/vagrant-kubernetes-3-node-cluster-centos/latest

    // Обновить образы виртуальных машин virtualbox
    $ vagrant box update

    // Запуск
    $ vagrant up

    $ vagrant status
    Current machine states:

    master.k8s                running (virtualbox)
    node1.k8s                 running (virtualbox)
    node2.k8s                 running (virtualbox)

<br/>

<!--

P.S.

    // Если нужно установить по какой-то причине старую версию kubernetes
    // В следующем каталоге лежат скрипты для установки kubernetes сluster (1.11.6)
    $ cd misc/vagrant-provisioning-by-version/

-->

<br/>

Если coredns-\* не будут стартовать, обновить файл kube-flannel.yml, скачав его по ссылке с сайта:

https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/

<br/>

### Копирование на хост конфиг файла, чтобы управлять кластером удаленно

Если kubectl не установлена, то <a href="/devops/containers/kubernetes/install/">вот</a>.

<br/>

    // Если каталог не создан
    $ mkdir -p ~/.kube

<br/>

    // Копируем конфиг файл
    // Пароль root: kubeadmin
    $ scp root@master:/etc/kubernetes/admin.conf ~/.kube/config

<br/>

    $ kubectl version --short
    Client Version: v1.18.1
    Server Version: v1.18.1

<br/>

    $ kubectl get nodes
    NAME         STATUS   ROLES    AGE     VERSION
    master.k8s   Ready    master   12m     v1.18.1
    node1.k8s    Ready    <none>   9m12s   v1.18.1
    node2.k8s    Ready    <none>   6m5s    v1.18.1

<br/>

### Получить дополнительную информацию по кластеру

    $ kubectl get cs
    NAME                 STATUS    MESSAGE             ERROR
    controller-manager   Healthy   ok
    scheduler            Healthy   ok
    etcd-0               Healthy   {"health":"true"}

<br/>

    $ kubectl get po -n kube-system
    NAME                                 READY   STATUS    RESTARTS   AGE
    coredns-66bff467f8-pwclq             1/1     Running   0          161m
    coredns-66bff467f8-s9k8w             1/1     Running   0          161m
    etcd-master.k8s                      1/1     Running   0          161m
    kube-apiserver-master.k8s            1/1     Running   0          161m
    kube-controller-manager-master.k8s   1/1     Running   0          161m
    kube-flannel-ds-amd64-vsk5p          1/1     Running   0          158m
    kube-flannel-ds-amd64-z4mdr          1/1     Running   0          161m
    kube-flannel-ds-amd64-zb9hq          1/1     Running   0          156m
    kube-proxy-gcg5d                     1/1     Running   0          161m
    kube-proxy-wl4qd                     1/1     Running   0          158m
    kube-proxy-xk2bf                     1/1     Running   0          156m
    kube-scheduler-master.k8s            1/1     Running   0          161m

<br/>

    $ kubectl cluster-info
    Kubernetes master is running at https://192.168.0.10:6443
    KubeDNS is running at https://192.168.0.10:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

<br/>

### Если нужно, чтобы кластер использовал гугловский DNS вместо resolv.conf на worker'ах

    $ kubectl edit cm -n kube-system coredns

<br/>

```
forward . /etc/resolv.conf
```

Меняем на

```
forward . 8.8.8.8:53
```

<br/>

Проверить, возможно можно подключившись к pod и выполнив внутри:

    nslookup kubernetes.default

<br/>

### <a href="/devops/containers/kubernetes/kubeadm/vagrant-centos7-3-node-kubernetes-cluster-error/">Если что-то не так</a>
