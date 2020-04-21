---
layout: page
title: Создание службы Nodeport
permalink: /devops/containers/kubernetes/minikube/svc/nodeport/
---

# Создание службы Nodeport

Обновлено:  
21.04.2020

<br/>

    $ mkdir ~/nodejs-cats-app && cd ~/nodejs-cats-app

<br/>

    $ minikube start

<br/>


```
$ cat <<EOF | kubectl apply -f -
apiVersion: apps/v1
kind: Deployment
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
        env: dev
    spec:
      containers:
      - name: nodejs-cats-app
        image: webmakaka/cats-app
        imagePullPolicy: Always
        ports:
        - containerPort: 8080
EOF
```

<br/>


```
$ cat <<EOF | kubectl apply -f -
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
EOF
```

<br/>

    port: 80 - хз для чего задаем.
    targetPort: 8080 - порт на котором работает приложение внутри pod. Или, наверное даже это из deployment.
    nodePort: 30123 - то к какому порту обращаться на этот под.


<br/>

    $ kubectl create -f nodejs-cats-app-svc-nodeport.yaml

<br/>

    $ kubectl get pods
    NAME                               READY   STATUS    RESTARTS   AGE
    nodejs-cats-app-774f89d47b-2tbrj   1/1     Running   0          61s
    nodejs-cats-app-774f89d47b-8hjrv   1/1     Running   0          61s
    nodejs-cats-app-774f89d47b-lwc85   1/1     Running   0          61s


<br/>

    $ kubectl get svc
    NAME                       TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
    nodejs-cats-app-nodeport   NodePort   10.109.227.32   <none>        80:30123/TCP   22s


<br/>

    // Если не используется профиль, удалить
    // Если не используется namespace, таке можно убрать -n default
    $ echo $(minikube --profile my-profile service nodejs-cats-app-nodeport -n default --url)
    http://192.168.99.113:30123


<br/>

При обращении по адресу запустилось приложение.

<br/>

    // Если понадобится удалить
    $ kubectl delete svc nodejs-cats-app-nodeport
    $ kubectl delete deployment nodejs-cats-app
