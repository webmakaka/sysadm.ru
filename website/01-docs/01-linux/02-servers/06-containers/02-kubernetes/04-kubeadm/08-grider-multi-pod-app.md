---
layout: page
title: Разворачиваем приложение из видео курса Stephen Grider Docker and Kubernetes The Complete Guide
permalink: /linux/servers/containers/kubernetes/kubeadm/grider-multi-pod-app/
---

# Разворачиваем приложение из видео курса [Stephen Grider] Docker and Kubernetes: The Complete Guide [2018, ENG]

<br/>

Делаю: 18.04.2019

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

    NAME                                      READY   STATUS    RESTARTS   AGE
    client-deployment-bfb978799-pbtdn         1/1     Running   0          2m55s
    client-deployment-bfb978799-t76p5         1/1     Running   0          2m55s
    client-deployment-bfb978799-vz7m9         1/1     Running   0          2m55s
    nfs-client-provisioner-67cd85d66d-blc55   1/1     Running   0          8m6s
    postgres-deployment-6d85895ff4-6bwlh      1/1     Running   0          2m54s
    redis-deployment-74bb8bb895-hc6m7         1/1     Running   0          2m54s
    server-deployment-79cdbdb6fb-47kll        1/1     Running   0          2m54s
    server-deployment-79cdbdb6fb-k7wgf        1/1     Running   0          2m54s
    server-deployment-79cdbdb6fb-zdb7q        1/1     Running   0          2m54s
    wroker-deployment-6f79d4f66b-rfmrm        1/1     Running   0          2m54s

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