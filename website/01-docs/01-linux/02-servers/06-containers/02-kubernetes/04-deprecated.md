---
layout: page
title: Какие-то попытки разобраться с kubectl и minikube
permalink: /linux/servers/containers/kubernetes/deprecated/
---

# Какие-то попытки разобраться с kubectl и minikube

[Инсталляция kubectl и minikube](/linux/servers/containers/kubernetes/install/)

<br/>
    
    $ minikube start
    
    -- веб приложение для удобства
    $ minikube dashboard
    
    
    -- Конфиг файл для подключения к Kubernetes cluster создается здесь
    $ cat ~/.kube/config

    -- Также его можно посмотреть командой
    $ kubectl config view


    $ kubectl cluster-info
    Kubernetes master is running at https://192.168.99.101:8443


      -- Get the token
      $ TOKEN=$(kubectl describe secret $(kubectl get secrets | grep default | cut -f1 -d ' ') | grep -E '^token' | cut -f2 -d':' | tr -d '\t' | tr -d " ")

      $ echo $TOKEN

      -- get the API server endpoint
      $ APISERVER=$(kubectl config view | grep https | cut -f 2- -d ":" | tr -d " ")


      $ echo $APISERVER

      $ curl $APISERVER --header "Authorization: Bearer $TOKEN" --insecure

<br/>

### Запуск контейренов

    $ kubectl run hello-minikube --image=gcr.io/google_containers/echoserver:1.4 --port=8080


    $ kubectl expose deployment hello-minikube --type=NodePort


    $ kubectl get pod
    NAME                              READY     STATUS    RESTARTS   AGE
    hello-minikube-64698d6ccf-t4t97   1/1       Running   0          7m


    $ kubectl get deployments

    $ kubectl get replicasets


    $ kubectl describe pod  hello-minikube-64698d6ccf-t4t97

    $  kubectl get nodes
    NAME       STATUS    ROLES     AGE       VERSION
    minikube   Ready     <none>    5d        v1.9.4

<br/>

    $ echo $(minikube service hello-minikube --url)
    http://192.168.99.101:31457

<br/>

    $ minikube service hello-minikube

<br/>

    $ curl $(minikube service hello-minikube --url)
    CLIENT VALUES:
    client_address=172.17.0.1
    command=GET
    real path=/
    query=nil
    request_version=1.1
    request_uri=http://192.168.99.100:8080/

    SERVER VALUES:
    server_version=nginx: 1.10.0 - lua: 10001

    HEADERS RECEIVED:
    accept=*/*
    host=192.168.99.100:31457
    user-agent=curl/7.35.0
    BODY:
    -no body in request-

<br/>

    $ minikube stop
    $ minikube delete

<br/>

### Deploy an application from a YAML file using kubectl.

    $ kubectl get deployments
    NAME             DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
    hello-minikube   1         1         1            1           8d

<br/>

    $ kubectl delete deployments hello-minikube

<br/>

    $ vi webserver.yaml

<br/>

```yaml1
apiVersion: apps/v1
kind: Deployment
metadata:
    name: webserver
    labels:
    app: nginx
spec:
    replicas: 3
    selector:
    matchLabels:
        app: nginx
    template:
    metadata:
        labels:
        app: nginx
    spec:
        containers:
        - name: nginx
        image: nginx:alpine
        ports:
        - containerPort: 80

```

<br/>

    $ kubectl create -f webserver.yaml

<br/>

    -- $ kubectl get rs
    $ kubectl get replicasets
    NAME                  DESIRED   CURRENT   READY     AGE
    webserver-b477df957   3         3         1         <invalid>

<br/>
    
    $ kubectl get pods
    NAME                        READY     STATUS    RESTARTS   AGE
    webserver-b477df957-lhv7s   1/1       Running   0          <invalid>
    webserver-b477df957-ndqfr   1/1       Running   0          <invalid>
    webserver-b477df957-sgtqw   1/1       Running   0          <invalid>

<br/>

    $ vi webserver-svc.yaml

<br/>
    
    apiVersion: v1
    kind: Service
    metadata:
      name: web-service
      labels:
        run: web-service
    spec:
      type: NodePort
      ports:
      - port: 80
        protocol: TCP
      selector:
        app: nginx

<br/>

    $ kubectl create -f webserver-svc.yaml

<br/>

    $ kubectl get svc
    NAME             TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
    hello-minikube   NodePort    10.107.89.57     <none>        8080:31457/TCP   8d
    hello-service    NodePort    10.108.241.207   <none>        8080:32762/TCP   7d
    kubernetes       ClusterIP   10.96.0.1        <none>        443/TCP          8d
    nginx            ClusterIP   10.101.145.50    <none>        80/TCP           2d
    nginx2           NodePort    10.108.68.197    <none>        80:30382/TCP     2d
    viz              NodePort    10.108.34.191    <none>        8080:31296/TCP   2d
    web-service      NodePort    10.100.142.145   <none>        80:32392/TCP     <invalid>

<br/>

    $ kubectl describe svc web-service
    Name:                     web-service
    Namespace:                default
    Labels:                   run=web-service
    Annotations:              <none>
    Selector:                 app=nginx
    Type:                     NodePort
    IP:                       10.100.142.145
    Port:                     <unset>  80/TCP
    TargetPort:               80/TCP
    NodePort:                 <unset>  32392/TCP
    Endpoints:                172.17.0.2:80,172.17.0.4:80,172.17.0.6:80
    Session Affinity:         None
    External Traffic Policy:  Cluster
    Events:                   <none>

<br/>

    $ minikube ip
    192.168.99.100

<br/>

    $ minikube service web-service

<br/>

### ConfigMaps and Secrets

https://www.youtube.com/watch?v=UpPnmvHwsjA

<br/>

### Create the ConfigMap

    $ kubectl create configmap my-config --from-literal=key1=value1 --from-literal=key2=value2

<br/>

    $ kubectl get configmaps my-config -o yaml

<br/>

    $ vi customer1-configmap.yaml

    apiVersion: v1
    kind: ConfigMap
    metadata:
      name: customer1
    data:
      TEXT1: Customer1_Company
      TEXT2: Welcomes You
      COMPANY: Customer1 Company Technology Pct. Ltd.

<br/>

    $ kubectl create -f customer1-configmap.yaml

    $ kubectl get configmaps customer1 -o yaml

    $ kubectl describe configmap customer1
    Name:         customer1
    Namespace:    default
    Labels:       <none>
    Annotations:  <none>

    Data
    ====
    TEXT2:
    ----
    Welcomes You
    COMPANY:
    ----
    Customer1 Company Technology Pct. Ltd.
    TEXT1:
    ----
    Customer1_Company
    Events:  <none>

<br/>

### Secrets

    $ kubectl create secret generic my-password --from-literal=password=mysqlpassword

<br/>

    $ kubectl get secret my-password
    NAME          TYPE      DATA      AGE
    my-password   Opaque    1         <invalid>

<br/>

### Create a Secret Manually

    $ echo mysqlpassword | base64
    bXlzcWxwYXNzd29yZAo=

    $ echo -n "bXlzcWxwYXNzd29yZAo=" | base64 --decode

    $ vi ./secret.yaml

    apiVersion: v1
    kind: Secret
    metadata:
        name: mysecret
    type: Opaque
    data:
        username: YWRtaW4=
        password: cEAxODg3MXcwcmQ=

<br/>

    $ kubectl create -f ./secret.yaml

<br/>

    $ kubectl get secret
    NAME                  TYPE                                  DATA      AGE
    db-user-pass          Opaque                                2         10m
    default-token-7c5ss   kubernetes.io/service-account-token   3         3h
    mysecret              Opaque                                2         <invalid>

<br/>

    // если нужно удалить
    $ kubectl delete secret db-user-pass

<br/>
        
    $ kubectl describe secrets/db-user-pass
    Name:         db-user-pass
    Namespace:    default
    Labels:       <none>
    Annotations:  <none>

    Type:  Opaque

    Data
    ====
    username.txt:  5 bytes
    password.txt:  11 bytes

<br/>

### Secrets from file

    $ cd ~/
    $ echo -n "admin" > ./username.txt
    $ echo -n "p@$$w0rd" > ./password.txt
    $ kubectl create secret generic db-user-pass --from-file=./username.txt --from-file=./password.txt
    secret "db-user-pass" created

<br/>

### Ingress

https://www.youtube.com/watch?v=Blmw3kTSHCs

Ingress, which is another method we can use to access our applications from the external world

With Ingress, users don't connect directly to a Service. Users reach the Ingress endpoint, and, from there, the request is forwarded to the respective Service.

<br/>

    $ minikube addons enable ingress

<br/>

    $ vi webserver-ingress.yaml

<br/>
    
```yaml1
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
    name: web-ingress
    namespace: default
spec:
    rules:
    - host: blue.example.com
    http:
        paths:
        - backend:
            serviceName: webserver-blue-svc
            servicePort: 80
    - host: green.example.com
    http:
        paths:
        - backend:
            serviceName: webserver-green-svc
            servicePort: 80

```

<br/>

    $ kubectl create -f webserver-ingress.yaml

<br/>

    $ cat /etc/hosts
    127.0.0.1        localhost
    ::1              localhost
    192.168.99.100   blue.example.com green.example.com

<br/>

    $ kubectl describe ingress web-ingress
```
