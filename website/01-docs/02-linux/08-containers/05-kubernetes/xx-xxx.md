---
layout: page
title: Kubernetes
permalink: /linux/containers/kubernetes/xx-xxx/
---

# Kubernetes

VirtualBox должен быть установлен


<br/>

### Install kubectl

https://kubernetes.io/docs/tasks/tools/install-kubectl/


    $ curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
    
    $ chmod +x ./kubectl
    $ sudo mv ./kubectl /usr/local/bin/kubectl
    


<br/>

### Install minikube


https://github.com/kubernetes/minikube


    $ curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/
    
    
    $ minikube start
    
    $ minikube dashboard
    
    $ kubectl run hello-minikube --image=gcr.io/google_containers/echoserver:1.4 --port=8080
    

    
    $ kubectl expose deployment hello-minikube --type=NodePort
    
<!-- $ /usr/local/bin/kubectl delete deployment hello-minikube -->

    $ kubectl get pod
    NAME                              READY     STATUS    RESTARTS   AGE
    hello-minikube-64698d6ccf-t4t97   1/1       Running   0          7m


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


<br/>

### Secrets

    $ cd ~/
    $ echo -n "admin" > ./username.txt
    $ echo -n "p@$$w0rd" > ./password.txt
    $ kubectl create secret generic db-user-pass --from-file=./username.txt --from-file=./password.txt
    secret "db-user-pass" created
    
<br/>
    
    $ kubectl get secret 
    NAME                  TYPE                                  DATA      AGE
    db-user-pass          Opaque                                2         42s
    default-token-7c5ss   kubernetes.io/service-account-token   3         3h

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

    $ echo -n "admin" | base64
    $ echo -n "p@$$w0rd" | base64
    $ echo -n "YWRtaW4=" | base64 --decode
    
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

### 12-Using Secrets in Applications
    
    $ cd helloworld
    

    
    
    
