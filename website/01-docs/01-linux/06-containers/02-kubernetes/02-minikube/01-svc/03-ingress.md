---
layout: page
title: Создание службы Ingress
permalink: /linux/containers/kubernetes/minikube/svc/ingress/
---

# Создание службы Ingress

Делаю:  
02.03.2019

Replica Set и NodePort уже созданы как <a href="/linux/containers/kubernetes/minikube/svc/nodeport/">здесь</a>

<br/>

### Включение функционала Ingress в Minikube

<br/>

    $ minikube addons list
    - ingress: disabled

    // Включение функционала Ingress в Minikube
    $ minikube addons enable ingress

    $ kubectl get po --all-namespaces | grep ingres
    kube-system   nginx-ingress-controller-7c66d668b-l7vdr   1/1

<br/>

### Запускаем приложение

<br/>

    $ vi nodejs-cats-app-svc-ingress.yaml

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: nodejs-cats-app-ingress
spec:
  rules:
  - host: nodejs-cats-app.example.com
    http:
      paths:
      - path: /
        backend:
          serviceName: nodejs-cats-app-nodeport
          servicePort: 80
```

<br/>

    $ kubectl create -f nodejs-cats-app-svc-ingress.yaml

<br/>

    $ kubectl get ingresses
    NAME                         HOSTS                            ADDRESS   PORTS   AGE
    nodejs-cats-app-ingress   nodejs-cats-app.example.com             80      7s

<br/>

    $ minikube ip
    192.168.99.105

<br/>

    # vi /etc/hosts

    192.168.99.105 nodejs-cats-app.example.com

<br/>

Подключаемся по адресу:

http://nodejs-cats-app.example.com

<br/>

    // Если не нужно, удалить
    $ kubectl delete ingress nodejs-cats-app-ingress
