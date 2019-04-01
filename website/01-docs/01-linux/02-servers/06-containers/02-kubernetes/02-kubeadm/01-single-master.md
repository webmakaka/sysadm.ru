---
layout: page
title: Vagrant Скрипты разворачивающие Single Master Kubernetes Cluster
permalink: /linux/servers/containers/kubernetes/kubeadm/single-master/
---

# Vagrant Скрипты разворачивающие Single Master Kubernetes Cluster

Делаю  
01.04.2019

<br/>

Предполагается что уже установлен VirtualBox, Vagrant, kubectl

https://sysadm.ru/linux/servers/containers/kubernetes/install/

<br/>

Разворачиваются 3 виртуалки по 2GB оперативной памяти.

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

    $ cd vagrant-provisioning/

    $ vagrant up

<br/>

    $ scp root@master:/etc/kubernetes/admin.conf ~/.kube/config

Пароль root: kubeadmin

<br/>

    $ kubectl get nodes
    NAME         STATUS   ROLES    AGE     VERSION
    master.k8s   Ready    master   7m50s   v1.14.0
    node1.k8s    Ready    <none>   4m51s   v1.14.0
    node2.k8s    Ready    <none>   104s    v1.14.0

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
