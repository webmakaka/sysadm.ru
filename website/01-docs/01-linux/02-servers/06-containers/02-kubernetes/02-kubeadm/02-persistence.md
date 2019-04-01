---
layout: page
title: Using Persistent Volumes and Claims in Kubernetes Cluster (начинаем разбираться)
permalink: /linux/servers/containers/kubernetes/kubeadm/persistence/
---

# Using Persistent Volumes and Claims in Kubernetes Cluster (начинаем разбираться)

<br/>

По материалам из видео индуса.

https://www.youtube.com/watch?v=YzaYqxW0wGs&list=PL34sAs7_26wNBRWM6BDhnonoA5FMERax0

<br/>

### HostPath (не для продауктового использования)

Смысл показать, что если данные хранятся на одной ноде, а потом кластер переключится на другую. То данные автоматически не перенесутся и у приложения не будет каких-либо данных.

Подготовили кластер как <a href="/linux/servers/containers/kubernetes/kubeadm/single-master/">здесь</a>.

<br/>

    $ cd ../yamls/

    $ ssh root@master

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
