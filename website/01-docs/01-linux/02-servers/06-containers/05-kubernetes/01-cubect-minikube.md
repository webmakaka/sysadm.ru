---
layout: page
title: Какие-то попытки разобраться с cubectl и minikube
permalink: /linux/servers/containers/kubernetes/cubect-minikube/
---

# Какие-то попытки разобраться с cubectl и minikube

[Инсталляция cubectl и minikube](/linux/servers/containers/kubernetes/minikube/cubect-minikube-installation/)

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

    -- если есть желание подключиться к Kubernetes Dashboard
    $ kubectl proxy

    -- Можно коннектиться
    http://127.0.0.1:8001/api/v1/namespaces/kube-system/services/kubernetes-dashboard:/proxy/#!/overview?namespace=default

<br/>
    
    -- api
    $ curl http://localhost:8001/
    {
      "paths": [
        "/api",
        "/api/v1",
        "/apis",
        "/apis/",
        "/apis/admissionregistration.k8s.io",
        "/apis/admissionregistration.k8s.io/v1alpha1",
        "/apis/admissionregistration.k8s.io/v1beta1",
        "/apis/apiextensions.k8s.io",
        "/apis/apiextensions.k8s.io/v1beta1",
        "/apis/apiregistration.k8s.io",
        "/apis/apiregistration.k8s.io/v1beta1",
        "/apis/apps",
        "/apis/apps/v1",
        "/apis/apps/v1beta1",
        "/apis/apps/v1beta2",
        "/apis/authentication.k8s.io",
        "/apis/authentication.k8s.io/v1",
        "/apis/authentication.k8s.io/v1beta1",
        "/apis/authorization.k8s.io",
        "/apis/authorization.k8s.io/v1",
        "/apis/authorization.k8s.io/v1beta1",
        "/apis/autoscaling",
        "/apis/autoscaling/v1",
        "/apis/autoscaling/v2beta1",
        "/apis/batch",
        "/apis/batch/v1",
        "/apis/batch/v1beta1",
        "/apis/batch/v2alpha1",
        "/apis/certificates.k8s.io",
        "/apis/certificates.k8s.io/v1beta1",
        "/apis/events.k8s.io",
        "/apis/events.k8s.io/v1beta1",
        "/apis/extensions",
        "/apis/extensions/v1beta1",
        "/apis/networking.k8s.io",
        "/apis/networking.k8s.io/v1",
        "/apis/policy",
        "/apis/policy/v1beta1",
        "/apis/rbac.authorization.k8s.io",
        "/apis/rbac.authorization.k8s.io/v1",
        "/apis/rbac.authorization.k8s.io/v1alpha1",
        "/apis/rbac.authorization.k8s.io/v1beta1",
        "/apis/scheduling.k8s.io",
        "/apis/scheduling.k8s.io/v1alpha1",
        "/apis/settings.k8s.io",
        "/apis/settings.k8s.io/v1alpha1",
        "/apis/storage.k8s.io",
        "/apis/storage.k8s.io/v1",
        "/apis/storage.k8s.io/v1alpha1",
        "/apis/storage.k8s.io/v1beta1",
        "/healthz",
        "/healthz/autoregister-completion",
        "/healthz/etcd",
        "/healthz/ping",
        "/healthz/poststarthook/apiservice-openapi-controller",
        "/healthz/poststarthook/apiservice-registration-controller",
        "/healthz/poststarthook/apiservice-status-available-controller",
        "/healthz/poststarthook/bootstrap-controller",
        "/healthz/poststarthook/ca-registration",
        "/healthz/poststarthook/generic-apiserver-start-informers",
        "/healthz/poststarthook/kube-apiserver-autoregistration",
        "/healthz/poststarthook/start-apiextensions-controllers",
        "/healthz/poststarthook/start-apiextensions-informers",
        "/healthz/poststarthook/start-kube-aggregator-informers",
        "/healthz/poststarthook/start-kube-apiserver-informers",
        "/logs",
        "/metrics",
        "/swagger-2.0.0.json",
        "/swagger-2.0.0.pb-v1",
        "/swagger-2.0.0.pb-v1.gz",
        "/swagger.json",
        "/swaggerapi",
        "/ui",
        "/ui/",
        "/version"
      ]

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

    $ kubectl delete deployments hello-minikube

<br/>

    $ vi webserver.yaml

<br/>
    
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


    $ kubectl get configmaps my-config -o yaml


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

    $ vi webserver-ingress.yaml

<br/>
    
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
    
<br/>
    
    $ kubectl create -f webserver-ingress.yaml

<br/>

    $ cat /etc/hosts
    127.0.0.1        localhost
    ::1              localhost
    192.168.99.100   blue.example.com green.example.com

<br/>

    $ kubectl describe ingress web-ingress
