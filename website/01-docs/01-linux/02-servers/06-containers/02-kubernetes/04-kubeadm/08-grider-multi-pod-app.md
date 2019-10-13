---
layout: page
title: Разворачиваем приложение из видео курса Stephen Grider Docker and Kubernetes The Complete Guide
permalink: /linux/servers/containers/kubernetes/kubeadm/grider-multi-pod-app/
---

# Разворачиваем приложение из видео курса [Stephen Grider] Docker and Kubernetes: The Complete Guide [2018, ENG]

<br/>

Делаю:  
13.10.2019

<br/>

### Что-то на backend не заработало! (Нужно разбираться)

<br/>

    $ kubectl version --short
    Client Version: v1.16.1
    Server Version: v1.16.1

<br/>

**Обращаю внимание, что используется:**  
nginxinc-kubernetes-ingress

<br/>

**Ссылка на github:**  
https://github.com/marley-nodejs/Docker-and-Kubernetes-The-Complete-Guide

<br/>

**Само приложение:**

![Docker and Kubernetes The Complete Guide](https://raw.githubusercontent.com/marley-nodejs/Docker-and-Kubernetes-The-Complete-Guide/master/img/pic-15-01.png "Docker and Kubernetes The Complete Guide"){: .center-image }

<br/>

Подготовили кластер и окружение как <a href="/linux/servers/containers/kubernetes/kubeadm/prepared-cluster/">здесь</a>.

Подняли Dynamic NFS как<a href="/linux/servers/containers/kubernetes/kubeadm/persistence/dynamic-nfs-provisioning/">здесь</a>.

<br/>

### Разворачиваем приложение

    $ kubectl create secret generic pgpassword --from-literal PGPASSWORD=12345asdf

    $ cd ~/tmp
    $ git clone https://github.com/marley-nodejs/Docker-and-Kubernetes-The-Complete-Guide.git

    $ cd ~/tmp/Docker-and-Kubernetes-The-Complete-Guide/14_A_Multi_Container_App_with_Kubernetes

    $ kubectl create -f .

    $ kubectl get pods
    NAME                                     READY   STATUS    RESTARTS   AGE
    client-deployment-5ccb9bf4d-6687s        1/1     Running   0          68s
    client-deployment-5ccb9bf4d-95d2l        1/1     Running   0          68s
    client-deployment-5ccb9bf4d-xkn4d        1/1     Running   0          68s
    nfs-client-provisioner-b48654857-tmdrm   1/1     Running   0          6m46s
    postgres-deployment-789f77969f-pn7lf     1/1     Running   0          68s
    redis-deployment-5f458546b8-nwdcr        1/1     Running   0          68s
    server-deployment-9c87878c7-424rj        1/1     Running   0          68s
    server-deployment-9c87878c7-d5mv2        1/1     Running   0          68s
    server-deployment-9c87878c7-vcfg9        1/1     Running   0          68s
    wroker-deployment-97554b959-4nqkw        1/1     Running   0          68s


<br/>

Установили и настроили HAProxy как <a href="/linux/servers/containers/kubernetes/kubeadm/ingress/haproxy/">здесь</a>.

<br/>

### [Создаем NginxInc Kubernetes Ingress контроллер](/linux/servers/containers/kubernetes/kubeadm/ingress/nginxinc-kubernets-ingress-install/)

<br/>

### Добавляем свой конфиг

<br/>

```
$ cat <<EOF | kubectl apply -f -
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: nginxinc-kubernetes-ingress
  annotations:
    nginx.org/rewrites: "serviceName=server-cluster-ip-service rewrite=/"
spec:
  rules:
  - host: nginx.example.com
    http:
      paths:
      - path: /
        backend:
          serviceName: client-cluster-ip-service
          servicePort: 3000
      - path: /api/
        backend:
          serviceName: server-cluster-ip-service
          servicePort: 5000
EOF

```

<br/>

### На клиенте

    # vi /etc/hosts
    192.168.0.5 nginx.example.com

<br/>

**Пробуем подключаться:**

    nginx.example.com
    nginx.example.com/api/
    nginx.example.com/api/values/current
    OK

<!-- $ curl http://192.168.0.5 -H 'Host:nginx.example.com' -->
