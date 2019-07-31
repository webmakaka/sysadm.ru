---
layout: page
title: Vagrant скрипты, разворачивающие готовый Single Master Kubernetes Cluster
permalink: /linux/servers/containers/kubernetes/kubeadm/prepared-cluster/
---

# Vagrant скрипты, разворачивающие готовый Single Master Kubernetes Cluster

Делаю  
01.08.2019


Предполагается что уже установлен <a href="/linux/servers/virtual/virtualbox/install/">VirtualBox</a>, <a href="/linux/servers/virtual/vagrant/install/ubuntu/">Vagrant</a>, <a href="/linux/servers/containers/kubernetes/install/">kubectl</a>.

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

    $ git clone https://bitbucket.org/sysadm-ru/kubernetes .

    // Скрипты для установки актуальной версии kubernetes сluster
    $ cd vagrant-provisioning/

    // Скрипты для установки kubernetes сluster (1.11.6)
    $ cd misc/vagrant-provisioning-by-version/

    // Обновить образы виртуальных машин virtualbox
    $ vagrant box update

    // Запуск
    $ vagrant up

<br/>

### Копирование на хост конфиг файла, чтобы управлять кластером удаленно

Если kubectl не установлена, то <a href="/linux/servers/containers/kubernetes/install/">вот</a>.

<br/>

    // Если каталог не создан
    $ mkdir -p ~/.kube/config

<br/>

    // Копируем конфиг файл
    // Пароль root: kubeadmin
    $ scp root@master:/etc/kubernetes/admin.conf ~/.kube/config

<br/>

    $ kubectl version --short
    Client Version: v1.15.1
    Server Version: v1.15.1

<br/>

    $ kubectl get nodes
    NAME         STATUS   ROLES    AGE     VERSION
    master.k8s   Ready    master   11m     v1.15.1
    node1.k8s    Ready    <none>   7m41s   v1.15.1
    node2.k8s    Ready    <none>   4m2s    v1.15.1


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
    coredns-5c98db65d4-rq5mm             1/1     Running   0          11m
    coredns-5c98db65d4-s8rvc             1/1     Running   0          11m
    etcd-master.k8s                      1/1     Running   0          10m
    kube-apiserver-master.k8s            1/1     Running   0          10m
    kube-controller-manager-master.k8s   1/1     Running   0          10m
    kube-flannel-ds-amd64-2w2n5          1/1     Running   0          8m16s
    kube-flannel-ds-amd64-t8mgd          1/1     Running   0          4m37s
    kube-flannel-ds-amd64-wzc6s          1/1     Running   0          11m
    kube-proxy-5lchz                     1/1     Running   0          11m
    kube-proxy-nh2bz                     1/1     Running   0          4m37s
    kube-proxy-vf2jg                     1/1     Running   0          8m16s
    kube-scheduler-master.k8s            1/1     Running   0          10m

<br/>

    $ kubectl cluster-info
    Kubernetes master is running at https://192.168.0.10:6443
    KubeDNS is running at https://192.168.0.10:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy