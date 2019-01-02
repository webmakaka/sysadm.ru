---
layout: page
title: Kuberneters на локальном хосте (cubectl и minikube)
permalink: /linux/servers/containers/kubernetes/localhost/sfas/
---

# Kuberneters на локальном хосте (cubectl и minikube)

<br/>

$ minikube start

$ minikube status

$ kubectl cluster-info
Kubernetes master is running at https://192.168.99.100:8443
KubeDNS is running at https://192.168.99.100:8443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy


https://hub.docker.com/u/stephengrider/


    $ kubectl apply -f client-pod.yaml
    $ kubectl apply -f client-node-port.yaml

    $ kubectl get pods
    NAME         READY   STATUS    RESTARTS   AGE
    client-pod   1/1     Running   0          1m

    $ kubectl get services
    NAME               TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
    client-node-port   NodePort    10.100.19.35   <none>        3050:31515/TCP   10m
    kubernetes         ClusterIP   10.96.0.1      <none>        443/TCP          2h


    $ minikube ip
    192.168.99.100

    http://192.168.99.100:31515/