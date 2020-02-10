---
layout: page
title: Kubernetes Pods, Replicasets & Deployments
permalink: /devops/containers/kubernetes/basics/pods-replicasets-deployments/
---

# Kubernetes Pods, Replicasets & Deployments

Делаю: 05.04.2019

<br/>

По материалам из видео индуса.

https://www.youtube.com/watch?v=deFfAUZpoxs&list=PL34sAs7_26wNBRWM6BDhnonoA5FMERax0&index=8

<br/>

Подготовили кластер и окружение как <a href="/devops/containers/kubernetes/kubeadm/prepared-cluster/">здесь</a>.

<br/>

### Запускаем Pod

    $ kubectl create -f https://bitbucket.org/sysadm-ru/kubernetes/raw/faf2f86a2c1bb82053c5aba9ea7c96463e4e61b0/yamls/1-nginx-pod.yaml

<br/>

    $ kubectl get events

<br/>

    $ kubectl describe pod nginx

<br/>

    $ kubectl delete pod nginx

<br/>

### Запускаем Replicasets

    $ kubectl create -f https://bitbucket.org/sysadm-ru/kubernetes/raw/faf2f86a2c1bb82053c5aba9ea7c96463e4e61b0/yamls/1-nginx-replicaset.yaml

<br/>

    $ kubectl get pods
    NAME                     READY   STATUS    RESTARTS   AGE
    nginx-replicaset-8zzdc   1/1     Running   0          39s
    nginx-replicaset-tbvtk   1/1     Running   0          2m47s

<br/>

    $ kubectl delete pod nginx-replicaset-tbvtk

<br/>

    $ kubectl delete replicaset nginx-replicaset

<br/>

### Запускаем Deployments

    $ kubectl create -f https://bitbucket.org/sysadm-ru/kubernetes/raw/faf2f86a2c1bb82053c5aba9ea7c96463e4e61b0/yamls/1-nginx-deployment.yaml

<br/>

    $ kubectl get all
    NAME                                READY   STATUS    RESTARTS   AGE
    pod/nginx-deploy-7db9fccd9b-2lr9n   1/1     Running   0          36s
    pod/nginx-deploy-7db9fccd9b-jt4tk   1/1     Running   0          36s

    NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
    service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   32m

    NAME                           READY   UP-TO-DATE   AVAILABLE   AGE
    deployment.apps/nginx-deploy   2/2     2            2           36s

    NAME                                      DESIRED   CURRENT   READY   AGE
    replicaset.apps/nginx-deploy-7db9fccd9b   2         2         2       36s

<br/>

    $ kubectl describe pod nginx-deploy-7db9fccd9b-2lr9n
    $ kubectl describe replicaset nginx-deploy-7db9fccd9b

<br/>

    $ kubectl get pods -l run=nginx
    NAME                            READY   STATUS    RESTARTS   AGE
    nginx-deploy-7db9fccd9b-2lr9n   1/1     Running   0          7m
    nginx-deploy-7db9fccd9b-jt4tk   1/1     Running   0          7m

<br/>

    $ kubectl describe pod nginx-deploy-7db9fccd9b-2lr9n | grep -i controlled
    Controlled By:      ReplicaSet/nginx-deploy-7db9fccd9b

<br/>

    $ kubectl describe replicaset nginx-deploy-7db9fccd9b | grep -i controlled
    Controlled By:  Deployment/nginx-deploy

<br/>

    $ kubectl scale deploy nginx-deploy --replicas=3

<br/>

    $ kubectl delete deploy nginx-deploy

<!--

### port-forward


kubectl run nginx --image nginx

kubectl get pods

kubectl port-forward nginx-sdfdf 8080:80

kubectl logs nginx-sdfdf

kubectl scale deploy nginx --replicas2


kubectl expose deployment nginx --type NodePort --port 80


kubectl describe svc nginx

-->
