---
layout: page
title: Dynamically NFS provisioning
permalink: /linux/servers/containers/kubernetes/kubeadm/persistence/dynamic-nfs-provisioning/
---

### Dynamically NFS provisioning

Делаю: 06.04.2019

<br/>

По материалам из видео индуса.

https://www.youtube.com/watch?v=AavnQzWDTEk&list=PL34sAs7_26wNBRWM6BDhnonoA5FMERax0&index=24

<br/>

![kubernetes NFS provisioning](/img/linux/servers/containers/kubernetes/kubeadm/persistence/NFS-provisioning.png "kubernetes NFS provisioning"){: .center-image }

<br/>

Подготовили кластер и окружение как <a href="/linux/servers/containers/kubernetes/kubeadm/prepared-cluster/">здесь</a>.

<br/>

Подготовили экспорт <a href="/linux/servers/containers/kubernetes/kubeadm/persistence/nfs/">NFS</a>

<br/>

### Подготавливаем кластер для работы с NFS. (выполняем команды на хост машине)

    $ kubectl create -f https://bitbucket.org/sysadm-ru/kubernetes/raw/faf2f86a2c1bb82053c5aba9ea7c96463e4e61b0/yamls/nfs-provisioner/rbac.yaml

<br/>

    $ kubectl get clusterrole,clusterrolebinding,role,rolebinding | grep nfs

<br/>

    $ rm -rf ~/tmp/k8s/dynamic-nfs-provisioning/ && mkdir -p ~/tmp/k8s/dynamic-nfs-provisioning/ && cd ~/tmp/k8s/dynamic-nfs-provisioning/

<!-- $ kubectl create -f https://bitbucket.org/sysadm-ru/kubernetes/raw/faf2f86a2c1bb82053c5aba9ea7c96463e4e61b0/yamls/nfs-provisioner/class.yaml -->

    $ curl -LJO https://bitbucket.org/sysadm-ru/kubernetes/raw/faf2f86a2c1bb82053c5aba9ea7c96463e4e61b0/yamls/nfs-provisioner/class.yaml

<br/>

    $ vi class.yaml

Делаем, что storageclass будет по умолчанию, добавив аннотацию

```
annotations:
  storageclass.kubernetes.io/is-default-class: "true"
```

Получаем:

```
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: managed-nfs-storage
  annotations:
    storageclass.kubernetes.io/is-default-class: "true"
provisioner: example.com/nfs
parameters:
  archiveOnDelete: "false"

```

<br/>

    $ kubectl create -f class.yaml

<br/>

    $ $ kubectl get storageclass
    NAME                            PROVISIONER       AGE
    managed-nfs-storage (default)   example.com/nfs   7s

<br/>

    $ curl -LJO https://bitbucket.org/sysadm-ru/kubernetes/raw/faf2f86a2c1bb82053c5aba9ea7c96463e4e61b0/yamls/nfs-provisioner/deployment.yaml

<br/>

    $ vi deployment.yaml

    <<NFS Server IP>> меняю на 192.168.0.6 (в 2 местах)

<br/>

    $ kubectl create -f deployment.yaml

<br/>

    $ kubectl get all
    NAME                                          READY   STATUS    RESTARTS   AGE
    pod/nfs-client-provisioner-67cd85d66d-sw4cm   1/1     Running   0          19s

    NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
    service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   41m

    NAME                                     READY   UP-TO-DATE   AVAILABLE   AGE
    deployment.apps/nfs-client-provisioner   1/1     1            1           19s

    NAME                                                DESIRED   CURRENT   READY   AGE
    replicaset.apps/nfs-client-provisioner-67cd85d66d   1         1         1       19s

<br/>

    $ kubectl get storageclass
    NAME                  PROVISIONER       AGE
    managed-nfs-storage   example.com/nfs   9m3s

<br/>

Подготовка закончена!

<br/>

## Изучаем всевозможные варианты использования

### Создаем PVC

    $ vi 4-pvc-nfs.yaml

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc1
spec:
  storageClassName: managed-nfs-storage
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 500Mi

```

<br/>

    $ kubectl create -f 4-pvc-nfs.yaml

<br/>

    $ kubectl get pv,pvc
    NAME                                                        CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM          STORAGECLASS          REASON   AGE
    persistentvolume/pvc-38a1eff2-5684-11e9-9ec5-525400261060   500Mi      RWX            Delete           Bound    default/pvc1   managed-nfs-storage            21s

    NAME                         STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS          AGE
    persistentvolumeclaim/pvc1   Bound    pvc-38a1eff2-5684-11e9-9ec5-525400261060   500Mi      RWX            managed-nfs-storage   31s

<br/>

    [vagrant@nfs-serv ~]$ ls /srv/nfs/kubedata/
    default-pvc1-pvc-38a1eff2-5684-11e9-9ec5-525400261060

<br/>

### Пробуем запустить контейнер

    $ vi 4-busybox-pv-hostpath.yaml

```

apiVersion: v1
kind: Pod
metadata:
  name: busybox
spec:
  volumes:
  - name: host-volume
    persistentVolumeClaim:
      claimName: pvc1
  containers:
  - image: busybox
    name: busybox
    command: ["/bin/sh"]
    args: ["-c", "sleep 600"]
    volumeMounts:
    - name: host-volume
      mountPath: /mydata

```

    $ kubectl create -f 4-busybox-pv-hostpath.yaml

<br/>

    $ kubectl get pods
    NAME                                      READY   STATUS    RESTARTS   AGE
    busybox                                   1/1     Running   0          12s
    nfs-client-provisioner-67cd85d66d-sw4cm   1/1     Running   0          15m

<br/>

Насрем в контейнере, как всегда и уйдем!

    $ kubectl exec -it busybox -- sh

    $ touch /mydata/hello

<br/>

    [vagrant@nfs-serv ~]$ ls /srv/nfs/kubedata/default-pvc1-pvc-38a1eff2-5684-11e9-9ec5-525400261060/
    hello

<br/>

    $ vi 4-pvc-nfs.yaml

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc2
spec:
  storageClassName: managed-nfs-storage
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 100Mi

```

    $ kubectl create -f 4-pvc-nfs.yaml

<br/>

    $ kubectl get pv,pvc
    NAME                                                        CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM          STORAGECLASS          REASON   AGE
    persistentvolume/pvc-38a1eff2-5684-11e9-9ec5-525400261060   500Mi      RWX            Delete           Bound    default/pvc1   managed-nfs-storage            10m
    persistentvolume/pvc-a399632e-5685-11e9-9ec5-525400261060   100Mi      RWO            Delete           Bound    default/pvc2   managed-nfs-storage            21s

    NAME                         STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS          AGE
    persistentvolumeclaim/pvc1   Bound    pvc-38a1eff2-5684-11e9-9ec5-525400261060   500Mi      RWX            managed-nfs-storage   10m
    persistentvolumeclaim/pvc2   Bound    pvc-a399632e-5685-11e9-9ec5-525400261060   100Mi      RWO            managed-nfs-storage   21s

<br/>

    [vagrant@nfs-serv ~]$ ls /srv/nfs/kubedata/
    default-pvc1-pvc-38a1eff2-5684-11e9-9ec5-525400261060
    default-pvc2-pvc-a399632e-5685-11e9-9ec5-525400261060

<br/>

### Удаляем, когда надоело играться

    $ kubectl delete pod busybox
    $ kubectl delete pvc --all
    $ kubectl delete deploy nfs-client-provisioner
