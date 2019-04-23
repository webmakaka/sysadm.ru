---
layout: page
title: Vagrant скрипты, разворачивающие готовый Single Master Kubernetes Cluster
permalink: /linux/servers/containers/kubernetes/kubeadm/prepared-cluster/
---

# Vagrant скрипты, разворачивающие готовый Single Master Kubernetes Cluster

Делаю  
24.04.2019

<br/>

Предполагается что уже установлен <a href="/linux/servers/virtual/virtualbox/install/">VirtualBox</a>, <a href="/linux/servers/virtual/vagrant/install/ubuntu/">Vagrant</a>, <a href="/linux/servers/containers/kubernetes/install/">kubectl</a>

<br/>

Разворачиваются 3 виртуалки по 2GB оперативной памяти. По идее, на узлы вполне достаточно и 1GB.

<br/>

### На хост машине

<br/>

    $ vagrant plugin install vagrant-hostmanager

<br/>

    $ sudo /etc/hosts

```
#---------------------------------------------------------------------
# Kubernetes cluster
#---------------------------------------------------------------------

192.168.0.10 master.k8s master
192.168.0.11 node1.k8s node1
192.168.0.12 node2.k8s node2
```

    $ mkdir ~/vagrant-kubernetes && cd ~/vagrant-kubernetes

    # git clone https://bitbucket.org/sysadm-ru/kubernetes .

    // Скрипты для установки актуальной версии kubernetes сluster
    $ cd vagrant-provisioning/

    // Скрипты для установки kubernetes сluster (1.11.6)
    $ cd misc/vagrant-provisioning-by-version/

    // Запуск
    $ vagrant up

<br/>

### Копирование на хост конфиг файла, чтобы управлять кластером удаленно

Если kubectl не установлена, то <a href="/linux/servers/containers/kubernetes/install/">вот</a>.

<br/>

    // Если каталог не создан
    $ mkdir -p ~/.kube/config

<br/>

    // Пароль root: kubeadmin
    $ scp root@master:/etc/kubernetes/admin.conf ~/.kube/config

<br/>

    $ kubectl version --short
    Client Version: v1.14.1
    Server Version: v1.14.1

<br/>

    $ kubectl get nodes
    NAME         STATUS   ROLES    AGE     VERSION
    master.k8s   Ready    master   14m     v1.14.1
    node1.k8s    Ready    <none>   11m     v1.14.1
    node2.k8s    Ready    <none>   9m34s   v1.14.1

<br/>

### Получить дополнительную информацию по кластеру

    $ kubectl get cs
    NAME                 STATUS    MESSAGE             ERROR
    scheduler            Healthy   ok
    controller-manager   Healthy   ok
    etcd-0               Healthy   {"health":"true"}

<br/>

    $ kubectl get po -n kube-system
    NAME                                 READY   STATUS    RESTARTS   AGE
    coredns-fb8b8dccf-74mcz              1/1     Running   0          10m
    coredns-fb8b8dccf-rpcdf              1/1     Running   0          10m
    etcd-master.k8s                      1/1     Running   0          9m48s
    kube-apiserver-master.k8s            1/1     Running   0          10m
    kube-controller-manager-master.k8s   1/1     Running   0          9m50s
    kube-flannel-ds-amd64-8vvqx          1/1     Running   0          4m53s
    kube-flannel-ds-amd64-b97h7          1/1     Running   0          10m
    kube-flannel-ds-amd64-m9qnt          1/1     Running   0          8m
    kube-proxy-f7fh2                     1/1     Running   0          10m
    kube-proxy-trvqn                     1/1     Running   0          8m
    kube-proxy-wfbsl                     1/1     Running   0          4m53s
    kube-scheduler-master.k8s            1/1     Running   0          9m47s

<br/>

    $ kubectl cluster-info
    Kubernetes master is running at https://192.168.0.10:6443
    KubeDNS is running at https://192.168.0.10:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
