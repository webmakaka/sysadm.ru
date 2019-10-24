---
layout: page
title: Скрипты, разворачивающие Single Master Kubernetes Cluster в VirtualBox
permalink: /linux/servers/containers/kubernetes/kubeadm/prepared-cluster/
---

# Скрипты, разворачивающие Single Master Kubernetes Cluster в VirtualBox

Делаю  
24.10.2019


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
    $ cd ~/vagrant-kubernetes/vagrant-provisioning/

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

P.S.

    // Если нужно установить по какой-то причине старую версию kubernetes
    // В следующем каталоге лежат скрипты для установки kubernetes сluster (1.11.6)
    $ cd misc/vagrant-provisioning-by-version/

<br/>

Если coredns-* не будут стартовать, обновить файл kube-flannel.yml, скачав его по ссылке с сайта:

https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/


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
    Client Version: v1.16.2
    Server Version: v1.16.2


<br/>

    $ kubectl get nodes
    NAME         STATUS   ROLES    AGE    VERSION
    master.k8s   Ready    master   13m    v1.16.2
    node1.k8s    Ready    <none>   10m    v1.16.2
    node2.k8s    Ready    <none>   7m3s   v1.16.2


<br/>

### Получить дополнительную информацию по кластеру

    $  kubectl get cs
    NAME                 AGE
    scheduler            <unknown>
    controller-manager   <unknown>
    etcd-0               <unknown>

<br/>

    $ kubectl get po -n kube-system
    NAME                                 READY   STATUS    RESTARTS   AGE
    coredns-5644d7b6d9-kj86n             1/1     Running   0          13m
    coredns-5644d7b6d9-pz26w             1/1     Running   0          13m
    etcd-master.k8s                      1/1     Running   0          13m
    kube-apiserver-master.k8s            1/1     Running   0          13m
    kube-controller-manager-master.k8s   1/1     Running   0          13m
    kube-flannel-ds-amd64-68r2l          1/1     Running   0          11m
    kube-flannel-ds-amd64-q6vhm          1/1     Running   2          7m40s
    kube-flannel-ds-amd64-zbjfx          1/1     Running   0          13m
    kube-proxy-bj6n6                     1/1     Running   0          11m
    kube-proxy-fg5rf                     1/1     Running   0          13m
    kube-proxy-t2dbc                     1/1     Running   0          7m40s
    kube-scheduler-master.k8s            1/1     Running   0          13m



<br/>

    $ kubectl cluster-info
    Kubernetes master is running at https://192.168.0.10:6443
    KubeDNS is running at https://192.168.0.10:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy


<br/>

### Если что-то не так

    $ kubectl get nodes
    NAME         STATUS     ROLES    AGE   VERSION
    master.k8s   NotReady   master   30m   v1.16.0
    node1.k8s    NotReady   <none>   26m   v1.16.0
    node2.k8s    NotReady   <none>   23m   v1.16.0

<br/>

, начинаем поиск

    $ kubectl describe nodes master.k8s


<br/>

```
Ready            False   Sun, 22 Sep 2019 10:27:17 +0300   Sun, 22 Sep 2019 09:56:54 +0300   KubeletNotReady              runtime network not ready: NetworkReady=false reason:NetworkPluginNotReady message:docker: network plugin is not ready: cni config uninitialized
```

<br/>


```
$ kubectl get pods --all-namespaces
NAMESPACE     NAME                                 READY   STATUS    RESTARTS   AGE
kube-system   coredns-5644d7b6d9-kgv52             0/1     Pending   0          43m
kube-system   coredns-5644d7b6d9-zlxbc             0/1     Pending   0          43m
kube-system   etcd-master.k8s                      1/1     Running   0          43m
kube-system   kube-apiserver-master.k8s            1/1     Running   0          42m
kube-system   kube-controller-manager-master.k8s   1/1     Running   0          42m
kube-system   kube-flannel-ds-amd64-95z5w          1/1     Running   0          43m
kube-system   kube-flannel-ds-amd64-97ssr          1/1     Running   0          37m
kube-system   kube-flannel-ds-amd64-xdj4c          1/1     Running   0          40m
kube-system   kube-proxy-8cjsl                     1/1     Running   0          37m
kube-system   kube-proxy-mp87t                     1/1     Running   0          43m
kube-system   kube-proxy-vdhvn                     1/1     Running   0          40m
kube-system   kube-scheduler-master.k8s            1/1     Running   0          43m
```

<br/>

    $ kubectl describe pod coredns.... -n kube-system
    $ kubectl delete pod coredns.... -n kube-system

<br/>

Описание решения данной ошибки:

https://stackoverflow.com/questions/58037620/how-to-fix-flannel-cni-plugin-error-plugin-flannel-does-not-support-config-ve