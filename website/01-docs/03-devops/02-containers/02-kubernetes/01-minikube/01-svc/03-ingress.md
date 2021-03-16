---
layout: page
title: Создание службы Ingress
description: Создание службы Ingress
keywords: devops, containers, kubernetes, minikube, Создание службы Ingress
permalink: /devops/containers/kubernetes/minikube/svc/ingress/
---

# Создание службы Ingress

Обновлено:  
12.02.2021

<br/>

Deployment и NodePort уже созданы как <a href="/devops/containers/kubernetes/minikube/svc/nodeport/">здесь</a>

<br/>

### Включение функционала Ingress в Minikube

<br/>

    $ minikube --profile my-profile addons list
    - ingress: disabled

    // Включение функционала Ingress в Minikube
    $ minikube addons --profile my-profile enable ingress

    $ kubectl get po --all-namespaces | grep ingres
    kube-system   nginx-ingress-controller-6fc5bcc8c9-rplp8   0/1     ContainerCreating   0          12s

<br/>

### Запускаем приложение

```yaml
$ cat <<EOF | kubectl apply -f -
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: nodejs-cats-app-ingress
spec:
  rules:
  - host: nodejs-cats-app.example.com
    http:
      paths:
      - path: /
        pathType: 'Prefix'
        backend:
          service:
            name: nodejs-cats-app-nodeport
            port:
              number: 80
EOF
```

<br/>

    $ kubectl get ingresses
    NAME                      HOSTS                         ADDRESS   PORTS   AGE
    nodejs-cats-app-ingress   nodejs-cats-app.example.com             80      23s

<br/>

    $ minikube --profile my-profile ip
    192.168.99.113

<br/>

```
$ sudo vi /etc/hosts

192.168.99.113 nodejs-cats-app.example.com
```

<br/>

Подключаемся по адресу:

http://nodejs-cats-app.example.com

<br/>

Все работает!

<br/>

    // Удалить ingress
    $ kubectl delete ingress nodejs-cats-app-ingress

    // И остальное
    $ kubectl delete svc nodejs-cats-app-nodeport
    $ kubectl delete deployment nodejs-cats-app
