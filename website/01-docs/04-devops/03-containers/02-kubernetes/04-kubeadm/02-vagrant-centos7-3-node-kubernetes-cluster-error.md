---
layout: page
title: Ошибки, возникавшие при работе с Single Master Kubernetes Cluster в VirtualBox
description: Ошибки, возникавшие при работе с Single Master Kubernetes Cluster в VirtualBox
keywords: devops, kubernetes, virtualbox, centos7, errors
permalink: /devops/containers/kubernetes/kubeadm/vagrant-centos7-3-node-kubernetes-cluster-error/
---

<br/>

# Ошибки, возникавшие при работе с Single Master Kubernetes Cluster в VirtualBox

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
