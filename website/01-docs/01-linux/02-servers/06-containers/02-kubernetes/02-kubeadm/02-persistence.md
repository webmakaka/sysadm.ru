---
layout: page
title: Using Persistent Volumes and Claims in Kubernetes Cluster (начинаем разбираться)
permalink: /linux/servers/containers/kubernetes/kubeadm/persistence/
---

# Using Persistent Volumes and Claims in Kubernetes Cluster (начинаем разбираться)

Делаю: 31.03.2019

<br/>

По материалам из видео индуса.

https://www.youtube.com/watch?v=I9GMUn15Nes&index=14&list=PL34sAs7_26wNBRWM6BDhnonoA5FMERax0

<br/>

### HostPath (не для продауктового использования)

Смысл показать, что если данные хранятся на одной ноде, а потом кластер переключится на другую. То данные автоматически не перенесутся и у приложения не будет каких-либо данных.

Подготовили кластер как <a href="/linux/servers/containers/kubernetes/kubeadm/single-master/">здесь</a>.

<br/>

    $ mkdir ~/vagrant-kubernetes-scripts && cd ~/vagrant-kubernetes-scripts

    # git clone https://bitbucket.org/sysadm-ru/kubernetes .


    $ cd yamls/

    $ ssh root@master

    Пароль root: kubeadmin

    # mkdir /kube
    # chmod 777 /kube/


    $ kubectl create -f 4-pv-hostpath.yaml

    $ kubectl get pv
    NAME          CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM   STORAGECLASS   REASON   AGE
    pv-hostpath   1Gi        RWO            Retain           Available           manual                  30s


    $ kubectl create -f 4-pvc-hostpath.yaml

    $ kubectl get pvc
    NAME           STATUS   VOLUME        CAPACITY   ACCESS MODES   STORAGECLASS   AGE
    pvc-hostpath   Bound    pv-hostpath   1Gi        RWO            manual         30s


    $ kubectl create -f 4-busybox-pv-hostpath.yaml

    $ kubectl describe pod busybox

    $ kubectl exec busybox touch /mydata/hello

    $ kubectl delete pod busybox


    $ kubectl get pv,pvc
    NAME                           CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                  STORAGECLASS   REASON   AGE
    persistentvolume/pv-hostpath   1Gi        RWO            Retain           Bound    default/pvc-hostpath   manual                  12m

    NAME                                 STATUS   VOLUME        CAPACITY   ACCESS MODES   STORAGECLASS   AGE
    persistentvolumeclaim/pvc-hostpath   Bound    pv-hostpath   1Gi        RWO            manual         9m47s


    $ kubectl delete pvc pvc-hostpath

    # ls /kube/


    $ kubectl create -f 4-pvc-hostpath.yaml

    $ kubectl delete pvc pvc-hostpath

    $ kubectl delete pv pv-hostpath
    persistentvolume "pv-hostpath" deleted

<br/>

## NFS Persistent Volume in Kubernetes Cluster

Делаю: 02.04.2019

По материалам индуса:

https://www.youtube.com/watch?v=to14wmNmRCI&list=PL34sAs7_26wNBRWM6BDhnonoA5FMERax0&index=21

<br/>

Поднимаю кластер заново.

<br/>

Добавляю еще 1 виртуалку на которой будет смонтирован еще 1 раздел.

    $ mkdir ~/vagrant-kubernetes-scripts && cd ~/vagrant-kubernetes-scripts

<br/>

    $ vi Vagrantfile

Виртуалка для NFS

```
# -*- mode: ruby -*-
# vi: set ft=ruby :

ENV['VAGRANT_NO_PARALLEL'] = 'yes'

Vagrant.configure(2) do |config|
  config.vm.box = "centos/7"
  config.hostmanager.enabled = true
  config.hostmanager.include_offline = true

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"
    vb.cpus = 2
  end

  config.vm.define "nfs-serv.k8s" do |c|
    c.vm.hostname = "nfs-serv.k8s"
    c.vm.network "private_network", ip: "192.168.0.6"
  end
end
```

<br/>

    $ vagrant up

    $ vagrant status
    Current machine states:

    nfs-serv.k8s              running (virtualbox)

    $ vagrant ssh nfs-serv.k8s

<br/>

### Внутри nfs-serv.k8s

    // пинг кластера
    $ ping 192.168.0.10
    ok

    $ sudo yum install nfs-utils

    $ sudo systemctl enable nfs-server
    $ sudo systemctl start nfs-server

    $ sudo mkdir -p /srv/nfs/kubedata
    $ sudo chmod -R 777 /srv/nfs

<br/>

Для демо, не для продуктового использования! Мне сейчас не до правильных настроек на тестовом сервере.

    $ sudo vi /etc/exports

    /srv/nfs/kubedata *(rw,sync,no_subtree_check,insecure)

На youtube под видео, индусы предлагают решение, чтобы не использовать insecure.

    $ sudo exportfs -rav

    $ sudo exportfs -v
    $ sudo showmount -e

    $ sudo vi /srv/nfs/kubedata/index.html

    <h1>NFS Server</h1>

<br/>

### На хост машине. Проверка что nfs монтируется на узлах

    // пароль kubeadmin
    $ ssh root@node1

    # showmount -e 192.168.0.6
    Export list for 192.168.0.6:
    /srv/nfs/kubedata *

    # mount -t nfs 192.168.0.6:/srv/nfs/kubedata /mnt/

    # mount | grep kubedata

    # ls /mnt/
    index.html

<!-- # umount /mnt -->

<br/>

    $ mkdir ~/tmp && cd ~/tmp/

    $ curl -LJO https://bitbucket.org/sysadm-ru/kubernetes/raw/faf2f86a2c1bb82053c5aba9ea7c96463e4e61b0/yamls/4-pv-nfs.yaml


    $ vi 4-pv-nfs.yaml

    server: 192.168.0.6

    $ kubectl create -f 4-pv-nfs.yaml

<br/>

    $ kubectl get pv
    NAME         CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                 STORAGECLASS   REASON   AGE
    pv-nfs-pv1   1Gi        RWX            Retain           Bound    default/pvc-nfs-pv1   manual                  18s

<br/>

    $ kubectl create -f https://bitbucket.org/sysadm-ru/kubernetes/raw/faf2f86a2c1bb82053c5aba9ea7c96463e4e61b0/yamls/4-pvc-nfs.yaml

<br/>

    $ kubectl get pv,pvc
    NAME                          CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                 STORAGECLASS   REASON   AGE
    persistentvolume/pv-nfs-pv1   1Gi        RWX            Retain           Bound    default/pvc-nfs-pv1   manual                  34s

    NAME                                STATUS   VOLUME       CAPACITY   ACCESS MODES   STORAGECLASS   AGE
    persistentvolumeclaim/pvc-nfs-pv1   Bound    pv-nfs-pv1   1Gi        RWX            manual         22s

<br/>

    $ kubectl create -f https://bitbucket.org/sysadm-ru/kubernetes/raw/faf2f86a2c1bb82053c5aba9ea7c96463e4e61b0/yamls/4-nfs-nginx.yaml

<br/>

    $ kubectl get pods
    NAME                            READY   STATUS    RESTARTS   AGE
    nginx-deploy-69bd64468b-w9zmv   1/1     Running   0          17s

<br/>

    $ kubectl exec -it nginx-deploy-69bd64468b-w9zmv -- /bin/sh

    $ cd  /usr/share/nginx/html
    $ cat index.html
    <h1>NFS Server</h1>

    $ exit

<br/>

    $ kubectl expose deploy nginx-deploy --port 80 --type NodePort

<br/>

    $ kubectl get svc nginx-deploy
    NAME           TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
    nginx-deploy   NodePort   10.100.31.163   <none>        80:30133/TCP   103s

<br/>

    $ curl node1:30133
    <h1>NFS Server</h1>

<br/>

### Удаление созданного на кластере

    $ kubectl delete svc nginx-deploy
    $ kubectl delete deploy nginx-deploy
    $ kubectl delete pvc pvc-nfs-pv1
    $ kubectl delete pv pv-nfs-pv1

============

1-nginx-daemonset.yaml

kubectl describe daemonsets nginx-daemonset

kubectl describe pod

kubectl delete daemonset nginx-daemonset
