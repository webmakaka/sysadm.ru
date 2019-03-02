---
layout: page
title: Создание службы Nodeport
permalink: /linux/servers/containers/kubernetes/minikube/svc/nodeport/
---

# Создание службы Nodeport

Делаю:  
02.03.2019

<br/>

<br/>

    $ cd ~
    $ mkdir tmp
    $ cd tmp/

<br/>

    $ minikube start

<br/>

    $ vi nodejs-voting-game-replicaset.yaml

```
apiVersion: apps/v1beta2
kind: ReplicaSet
metadata:
  name: nodejs-voting-game
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nodejs-voting-game
  template:
    metadata:
      labels:
        app: nodejs-voting-game
    spec:
      containers:
      - name: nodejs-voting-game
        image: marley/nodejs-voting-game
```

    $ kubectl create -f nodejs-voting-game-replicaset.yaml

<br/>

    $ vi nodejs-voting-game-svc-nodeport.yaml

```
apiVersion: v1
kind: Service
metadata:
  name: nodejs-voting-game-nodeport
spec:
  type: NodePort
  ports:
  - port: 80
    targetPort: 8080
    nodePort: 30123
  selector:
    app: nodejs-voting-game
```

    $ kubectl create -f nodejs-voting-game-svc-nodeport.yaml

<br/>

    $ kubectl get pods
    NAME                       READY   STATUS    RESTARTS   AGE
    nodejs-voting-game-5z649   1/1     Running   0          35s
    nodejs-voting-game-9kfc6   1/1     Running   0          35s
    nodejs-voting-game-wsll5   1/1     Running   0          35s

<br/>

    $ kubectl get svc
    NAME                          TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
    kubernetes                    ClusterIP   10.96.0.1      <none>        443/TCP        5m20s
    nodejs-voting-game-nodeport   NodePort    10.111.60.77   <none>        80:30123/TCP   31s

<br/>

    $ echo $(minikube service nodejs-voting-game-nodeport --url)
    http://192.168.99.105:30123

<br/>

При обращении по адресу запустилось приложение.

<br/>

    // Если понадобится удалить
    $ kubectl delete svc nodejs-voting-game-nodeport
