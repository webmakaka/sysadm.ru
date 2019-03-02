---
layout: page
title: Создание службы Ingress
permalink: /linux/servers/containers/kubernetes/minikube/svc/ingress/
---

# Создание службы Ingress

Делаю:  
02.03.2019

Replica Set созданы как в NodePort
NodePort создан как в NodePort

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

    $ vi nodejs-voting-game-svc-ingress.yaml

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: nodejs-voting-game-ingress
spec:
  rules:
  - host: nodejs-voting-game.example.com
    http:
      paths:
      - path: /
        backend:
          serviceName: nodejs-voting-game-nodeport
          servicePort: 80
```

<br/>

    $ kubectl create -f nodejs-voting-game-svc-ingress.yaml

<br/>

    $ kubectl get ingresses
    NAME                         HOSTS                            ADDRESS   PORTS   AGE
    nodejs-voting-game-ingress   nodejs-voting-game.example.com             80      7s

<br/>

    $ minikube ip
    192.168.99.105

<br/>

    # vi /etc/hosts

    192.168.99.105 nodejs-voting-game.example.com

<br/>

Подключаемся по адресу nodejs-voting-game.example.com

    // Если не нужно, удалить
    $ kubectl delete ingress nodejs-voting-game-ingress
