---
layout: page
title: Istio Request Routing
description: Istio Request Routing
keywords: linux, kubernetes, Istio, Request Routing
permalink: /devops/containers/kubernetes/service-mesh/istio/request-routing/
---

# Istio Request Routing


Поднята виртуальная машина с minikube <a href="/devops/containers/kubernetes/service-mesh/istio/minikube/env/">следующим образом</a>.

<br/>

Делаю:  
17.04.2020


https://www.youtube.com/watch?v=a0Mu0hQ9zzI

https://github.com/carnage-sh/cloud-for-fun/tree/master/blog/istio-routing



<br/>

    $ cd ~/
    $ git clone https://github.com/carnage-sh/cloud-for-fun/
    $ cd cloud-for-fun/blog/istio-routing

<br/>

    $ kubectl convert -f helloworld-v1.yaml > helloworld-v1.1.yaml

    $ kubectl apply -f helloworld-v1.1.yaml
    $ kubectl apply -f helloworld-gateway-v1.yaml

<br/>


    $ kubectl convert -f helloworld-v2.yaml > helloworld-v2.1.yaml

    $ kubectl apply -f helloworld-v2.1.yaml
    $ kubectl apply -f helloworld-gateway-v2.yaml

<br/>

    $ kubectl get service -n istio-system istio-ingressgateway
    EXTERNAL-IP --> 192.168.99.96

<br/>

    $ curl 192.168.99.96
    Hello version: v1, instance: helloworld-v1-7695cb4556-9dnsx

<br/>

    $ curl 192.168.99.96 -H 'x-user: gregory'
    Hello version: v2, instance: helloworld-v2-58b576ddf4-49zds

<br/>

В общем для "gregory" особый сервис, не такой как у всех. Ему повезло, он не такой как все.

<br/>


    $ kubectl get vs
    NAME         GATEWAYS               HOSTS   AGE
    helloworld   [helloworld-gateway]   [*]     3m44s


<br/>

    $ kubectl get svc
    NAME            TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
    helloworld-v1   ClusterIP   10.101.16.86     <none>        5000/TCP   6m10s
    helloworld-v2   ClusterIP   10.110.180.253   <none>        5000/TCP   3m53s
    kubernetes      ClusterIP   10.96.0.1        <none>        443/TCP    7m56s
