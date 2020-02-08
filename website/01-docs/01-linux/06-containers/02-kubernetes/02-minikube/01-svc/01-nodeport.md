---
layout: page
title: Создание службы Nodeport
permalink: /linux/containers/kubernetes/minikube/svc/nodeport/
---

# Создание службы Nodeport

Делаю:  
02.03.2019

<br/>

    $ mkdir ~/nodejs-cats-app && cd ~/nodejs-cats-app

<br/>

    $ minikube start

<br/>

    $ vi nodejs-cats-app-replicaset.yaml

```
apiVersion: apps/v1beta2
kind: ReplicaSet
metadata:
  name: nodejs-cats-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nodejs-cats-app
  template:
    metadata:
      labels:
        app: nodejs-cats-app
    spec:
      containers:
      - name: nodejs-cats-app
        image: marley/nodejs-cats-app
```

    $ kubectl create -f nodejs-cats-app-replicaset.yaml

<br/>

    $ vi nodejs-cats-app-svc-nodeport.yaml

```
apiVersion: v1
kind: Service
metadata:
  name: nodejs-cats-app-nodeport
spec:
  type: NodePort
  ports:
  - port: 80
    targetPort: 8080
    nodePort: 30123
  selector:
    app: nodejs-cats-app
```

<br/>

    $ kubectl create -f nodejs-cats-app-svc-nodeport.yaml

<br/>

    $ kubectl get pods
    NAME                       READY   STATUS    RESTARTS   AGE
    nodejs-cats-app-5z649   1/1     Running   0          35s
    nodejs-cats-app-9kfc6   1/1     Running   0          35s
    nodejs-cats-app-wsll5   1/1     Running   0          35s

<br/>

    $ kubectl get svc
    NAME                          TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
    kubernetes                    ClusterIP   10.96.0.1      <none>        443/TCP        5m20s
    nodejs-cats-app-nodeport   NodePort    10.111.60.77   <none>        80:30123/TCP   31s

<br/>

    $ echo $(minikube service nodejs-cats-app-nodeport --url)
    http://192.168.99.105:30123

<br/>

При обращении по адресу запустилось приложение.

<br/>

    // Если понадобится удалить
    $ kubectl delete svc nodejs-cats-app-nodeport
    $ kubectl delete rs nodejs-cats-app
