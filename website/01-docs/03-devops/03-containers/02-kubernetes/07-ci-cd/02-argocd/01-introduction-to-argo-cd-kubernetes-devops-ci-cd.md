---
layout: page
title: Introduction to Argo CD Kubernetes DevOps CI CD
description: Introduction to Argo CD Kubernetes DevOps CI CD
keywords: linux, kubernetes, ArgoCD
permalink: /devops/containers/kubernetes/ci-cd/argocd/introduction-to-argo-cd-kubernetes-devops-ci-cd/
---

# Introduction to Argo CD Kubernetes DevOps CI/CD

<br/>

Делаю:  
12.02.2021

<br/>

https://www.youtube.com/watch?v=2WSJF7d8dUg

<br/>

https://github.com/marcel-dempers/docker-development-youtube-series/tree/master/argo

<br/>

https://argo-cd.readthedocs.io/en/stable/getting_started/

<br/>

```
$ kubectl create namespace argocd
$ kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

<br/>

```
$ kubectl -n argocd get pods
NAME                                 READY   STATUS    RESTARTS   AGE
argocd-application-controller-0      1/1     Running   0          15m
argocd-dex-server-5fbb579948-pt9n5   1/1     Running   0          15m
argocd-redis-6fb68d9df5-vkb8t        1/1     Running   0          15m
argocd-repo-server-b4c6dc8f9-js262   1/1     Running   0          15m
argocd-server-56ffccb4cd-n9kfb       1/1     Running   0          15m
```

<br/>

```
$ kubectl port-forward svc/argocd-server -n argocd 8080:443
```

<br/>

### Получаем пароль

```
$ kubectl get pods -n argocd -l app.kubernetes.io/name=argocd-server -o name | cut -d'/' -f 2
```

<br/>

localhost:8080

admin / результат выполнения команды выше.

<br/>

```
$ cd ~/tmp/
$ git clone https://github.com/marcel-dempers/docker-development-youtube-series
$ cd docker-development-youtube-series/argo/
$ kubectl apply -n argocd -f ./argo-cd/app.yaml
```
