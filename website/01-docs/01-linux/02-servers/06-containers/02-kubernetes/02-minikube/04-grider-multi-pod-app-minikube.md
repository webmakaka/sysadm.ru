---
layout: page
title: Разворачиваем приложение из видео курса Stephen Grider Docker and Kubernetes The Complete Guide
permalink: /linux/servers/containers/kubernetes/minikube/grider-multi-pod-app-minikube/
---

# Разворачиваем приложение из видео курса [Stephen Grider] Docker and Kubernetes: The Complete Guide [2018, ENG]

<br/>

Делаю: 17.04.2019

<br/>

Этот вариант будет работать (предположительно) только на minikube.

<br/>

**Ссылка на github:**

https://github.com/marley-nodejs/Docker-and-Kubernetes-The-Complete-Guide

<br/>

**Само приложение:**

![Docker and Kubernetes The Complete Guide](https://raw.githubusercontent.com/marley-nodejs/Docker-and-Kubernetes-The-Complete-Guide/master/img/pic-15-01.png "Docker and Kubernetes The Complete Guide"){: .center-image }

<br/>

    $ minikube start
    $ minikube addons enable ingress

<br/>

### Разворачиваем приложение

    $ kubectl create secret generic pgpassword --from-literal PGPASSWORD=12345asdf

    $ cd ~/tmp
    $ git clone https://github.com/marley-nodejs/Docker-and-Kubernetes-The-Complete-Guide.git

    $ cd ~/tmp/Docker-and-Kubernetes-The-Complete-Guide/14_A_Multi_Container_App_with_Kubernetes

    $ kubectl create -f .

    $ kubectl get pods
    NAME                                   READY   STATUS    RESTARTS   AGE
    client-deployment-7677fdbc66-259wz     1/1     Running   0          66s
    client-deployment-7677fdbc66-7tv6d     1/1     Running   0          66s
    client-deployment-7677fdbc66-czqbk     1/1     Running   0          66s
    postgres-deployment-79d45ff857-w2ffn   1/1     Running   0          66s
    redis-deployment-6f6947dd7d-4d6vc      1/1     Running   0          66s
    server-deployment-74b6f7844f-5pp7w     1/1     Running   0          65s
    server-deployment-74b6f7844f-fhqkv     1/1     Running   0          65s
    server-deployment-74b6f7844f-krn5n     1/1     Running   0          65s
    wroker-deployment-864db9dddc-wtr82     1/1     Running   0          65s

<br/>

### KubernetesInc Ingress Nginx

    $ kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/mandatory.yaml

    $ cd ~/tmp/Docker-and-Kubernetes-The-Complete-Guide/15_Handling_Traffic_with_Ingress_Controllers/

    $ kubectl apply -f ingress-service.yaml

    $ minikube ip
    192.168.99.120

<br/>

**Ожидаемый результат:**

![Docker and Kubernetes The Complete Guide](https://raw.githubusercontent.com/marley-nodejs/Docker-and-Kubernetes-The-Complete-Guide/master/img/pic-15-05.png "Docker and Kubernetes The Complete Guide"){: .center-image }

<br/>

### Удаляем, если не нужно

    $ minikube stop
    $ minikube delete
