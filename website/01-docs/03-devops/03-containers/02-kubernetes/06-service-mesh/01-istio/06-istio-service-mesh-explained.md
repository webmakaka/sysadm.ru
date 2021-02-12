---
layout: page
title: Istio Service mesh explained
description: Istio Service mesh explained
keywords: devops, containers, kubernetes, service-mesh, istio
permalink: /devops/containers/kubernetes/service-mesh/istio/istio-service-mesh-explained/
---

# [That DevOps Guy] Istio Service mesh explained

<br/>

Делаю:  
12.02.2021

<br/>

**Не заработало! Нужно разбираться!**

Есил сделат так:

```
kubectl -n ingress-nginx port-forward deploy/nginx-ingress-controller 8080:80
```

Только первая страница подгружается.

<br/>

**YouTube**

https://www.youtube.com/watch?v=KUHzxTCe5Uc

<br/>

**GitHub**

https://github.com/marcel-dempers/docker-development-youtube-series

<br/>

**Дока**

https://github.com/marcel-dempers/docker-development-youtube-series/tree/master/kubernetes/servicemesh/istio

<br/>

```
$ cd ~/tmp
$ git clone https://github.com/marcel-dempers/docker-development-youtube-series
$ cd docker-development-youtube-series/
```

<br/>

**ingress controller**

```
$ kubectl create ns ingress-nginx
$ kubectl apply -f kubernetes/servicemesh/applications/ingress-nginx/
```

<br/>

**applications**

```
$ kubectl apply -f kubernetes/servicemesh/applications/playlists-api/
$ kubectl apply -f kubernetes/servicemesh/applications/playlists-db/
$ kubectl apply -f kubernetes/servicemesh/applications/videos-api/
$ kubectl apply -f kubernetes/servicemesh/applications/videos-web/
$ kubectl apply -f kubernetes/servicemesh/applications/videos-db/
```

<br/>

```
$ kubectl get pods
NAME                             READY   STATUS    RESTARTS   AGE
playlists-api-684fb4c5d7-5kvhm   2/2     Running   0          84s
playlists-db-7d68bbf5c4-jtlq8    2/2     Running   0          70s
videos-api-ccc8f5b46-sbdhc       2/2     Running   0          65s
videos-db-85ccd8bc-mq8rg         2/2     Running   0          51s
videos-web-cdbc466f4-s5dnr       2/2     Running   0          58s

```

<br/>

```
$ kubectl -n ingress-nginx get pods
NAME                                        READY   STATUS    RESTARTS   AGE
nginx-ingress-controller-77d9c88769-62wzn   1/1     Running   0          2m54s
nginx-ingress-controller-77d9c88769-rm6mg   1/1     Running   0          2m54s
```

<br/>

```
$ kubectl get ing
NAME            CLASS    HOSTS              ADDRESS   PORTS   AGE
playlists-api   <none>   servicemesh.demo             80      21m
videos-web      <none>   servicemesh.demo             80      20m
```

<br/>

```
$ echo EXTERNAL-IP=$(kubectl --namespace istio-system get svc istio-ingressgateway --template="{{range .status.loadBalancer.ingress}}{{.ip}}{{end}}")
```

<br/>

```
$ sudo vi /etc/hosts
```

```
192.168.49.20  servicemesh.demo
```

<br/>

http://servicemesh.demo/home/

<br/>

```
kubectl -n ingress-nginx port-forward deploy/nginx-ingress-controller 8080:80
```

<br/>

http://servicemesh.demo:8080/home/
