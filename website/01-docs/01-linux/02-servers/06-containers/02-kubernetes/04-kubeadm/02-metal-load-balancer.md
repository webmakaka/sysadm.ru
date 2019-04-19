---
layout: page
title: MetalLB Load Balancer in Kubernetes
permalink: /linux/servers/containers/kubernetes/kubeadm/metal-load-balancer/
---

# MetalLB Load Balancer in Kubernetes

Делаю: 18.04.2019

<br/>

По материалам из видео индуса.

https://www.youtube.com/watch?v=xYiYIjlAgHY&list=PL34sAs7_26wNBRWM6BDhnonoA5FMERax0&index=34

<br/>

Подготовили кластер и окружение как <a href="/linux/servers/containers/kubernetes/kubeadm/prepared-cluster/">здесь</a>.

<br/>

### Устанавливаем MetalLB

https://metallb.universe.tf/installation/

<br/>

    $ kubectl apply -f https://raw.githubusercontent.com/google/metallb/v0.7.3/manifests/metallb.yaml

<br/>

https://metallb.universe.tf/configuration/

<br/>

```

$ cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: ConfigMap
metadata:
  namespace: metallb-system
  name: config
data:
  config: |
    address-pools:
    - name: default
      protocol: layer2
      addresses:
      - 192.168.0.20-192.168.0.50
EOF

```

<br/>

Дождаться когда speaker будут в READY

<br/>

    $ kubectl get all -n metallb-system
    NAME                             READY   STATUS    RESTARTS   AGE
    pod/controller-cd8657667-h7flh   1/1     Running   0          10m
    pod/speaker-5g5cv                1/1     Running   0          10m
    pod/speaker-b7nd7                1/1     Running   0          10m

    NAME                     DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
    daemonset.apps/speaker   2         2         2       2            2           <none>          10m

    NAME                         READY   UP-TO-DATE   AVAILABLE   AGE
    deployment.apps/controller   1/1     1            1           10m

    NAME                                   DESIRED   CURRENT   READY   AGE
    replicaset.apps/controller-cd8657667   1         1         1       10m

<br/>

    // логи если что
    $ kubectl describe pod speaker-5g5cv -n metallb-system

<br/>

### Запуск

    $ kubectl run nginx --image nginx
    $ kubectl run nginx2 --image nginx

<br/>

    $ kubectl expose deploy nginx --port 80 --type LoadBalancer
    $ kubectl expose deploy nginx2 --port 80 --type LoadBalancer

<br/>

    $ kubectl get svc
    NAME                     TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)        AGE
    docker-hello-world-svc   ClusterIP      10.97.30.103     <none>         8088/TCP       30m
    kubernetes               ClusterIP      10.96.0.1        <none>         443/TCP        43m
    nginx                    LoadBalancer   10.101.185.186   192.168.0.20   80:30181/TCP   68s
    nginx2                   LoadBalancer   10.100.94.249    192.168.0.21   80:31269/TCP   62s

<br/>

    $ curl 192.168.0.20
    $ curl 192.168.0.21
    OK

<br/>

### Еще одно приложение

    $ kubectl run hello-world --replicas=5 --labels="run=load-balancer-example" --image=gcr.io/google-samples/node-hello:1.0 --port=8080

    $ kubectl expose deployment hello-world --type=LoadBalancer --name=my-service

    $ kubectl get services my-service
    NAME         TYPE           CLUSTER-IP      EXTERNAL-IP    PORT(S)          AGE
    my-service   LoadBalancer   10.106.38.188   192.168.0.27   8080:32482/TCP   43s

    $ curl 192.168.0.27:8080
    OK

<br/>

### Еще одно приложение на 8080 порту

    $ kubectl run cats-app-xxx2 --replicas=5 --labels="run=load-balancer-example" --image=marley/nodejs-cats-app:latest

    $ kubectl expose deployment cats-app-xxx2 --type=LoadBalancer --name=cats-app-xxx2-service --port=8080

    $ kubectl get services cats-app-xxx2-serviceNAME                    TYPE           CLUSTER-IP     EXTERNAL-IP    PORT(S)          AGE
    cats-app-xxx2-service   LoadBalancer   10.104.167.4   192.168.0.29   8080:32421/TCP   58s

    $ http://192.168.0.29:8080

<br/>

### Еще одно приложение на 80 порту

    $ kubectl run cats-app-m3 --replicas=5 --labels="run=load-balancer-example" --image=marley/nodejs-cats-app:latest

    $ kubectl expose deployment cats-app-m3 --type=LoadBalancer --name=cats-app-m3-service --target-port=8080 --port=80

    $ kubectl get services cats-app-m3-service

    $ http://192.168.0.38/

<br/>

### Еще одно приложение

```
$ cat <<EOF | kubectl apply -f -
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: docker-hello-world
  labels:
    app: docker-hello-world
spec:
  replicas: 3
  template:
    metadata:
      labels:
        app: docker-hello-world
    spec:
      containers:
      - name: docker-hello-world
        image: scottsbaldwin/docker-hello-world:latest
        ports:
        - containerPort: 80
EOF

```

<br/>

    $ kubectl get deploy
    NAME                 READY   UP-TO-DATE   AVAILABLE   AGE
    docker-hello-world   3/3     3            3           30s

<br/>

    $ kubectl expose deploy docker-hello-world --port 80 --type LoadBalancer

<br/>

    $ kubectl get svc docker-hello-world
    NAME                 TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)        AGE
    docker-hello-world   LoadBalancer   10.106.144.165   192.168.0.20   80:30502/TCP   25s

<br/>

    $ curl 192.168.0.20
    <h1>Hello webhook world from: docker-hello-world-6c88876fcd-x89cj</h1>

<br/>

    $ kubectl edit svc docker-hello-world

```
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: "2019-04-18T14:05:22Z"
  labels:
    app: docker-hello-world
  name: docker-hello-world
  namespace: default
  resourceVersion: "1376"
  selfLink: /api/v1/namespaces/default/services/docker-hello-world
  uid: 017f1cb2-61e3-11e9-8231-525400261060
spec:
  clusterIP: 10.106.144.165
  externalTrafficPolicy: Cluster
  ports:
  - nodePort: 30502
    port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: docker-hello-world
  sessionAffinity: None
  type: LoadBalancer
status:
  loadBalancer:
    ingress:
    - ip: 192.168.0.20
```

<br/>

### Еще одно приложение

```
$ cat <<EOF | kubectl apply -f -
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: cats-app-yyy2
  labels:
    app: cats-app-yyy2
spec:
  replicas: 3
  template:
    metadata:
      labels:
        app: cats-app-yyy2
    spec:
      containers:
      - name: cats-app-yyy2
        image: marley/nodejs-cats-app:latest
        ports:
        - containerPort: 8080
EOF

```

<br/>

    $ kubectl get deploy
    NAME                 READY   UP-TO-DATE   AVAILABLE   AGE
    docker-hello-world   3/3     3            3           30s

<br/>

    $ kubectl expose deploy cats-app-yyy2 --port 8080 --type LoadBalancer

<br/>

    $ kubectl get svc docker-hello-world
    NAME                 TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)        AGE
    docker-hello-world   LoadBalancer   10.106.144.165   192.168.0.20   80:30502/TCP   25s

<br/>

    $ curl 192.168.0.20
    <h1>Hello webhook world from: docker-hello-world-6c88876fcd-x89cj</h1>

<br/>

    $ kubectl edit svc docker-hello-world

```
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: "2019-04-18T14:05:22Z"
  labels:
    app: docker-hello-world
  name: docker-hello-world
  namespace: default
  resourceVersion: "1376"
  selfLink: /api/v1/namespaces/default/services/docker-hello-world
  uid: 017f1cb2-61e3-11e9-8231-525400261060
spec:
  clusterIP: 10.106.144.165
  externalTrafficPolicy: Cluster
  ports:
  - nodePort: 30502
    port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: docker-hello-world
  sessionAffinity: None
  type: LoadBalancer
status:
  loadBalancer:
    ingress:
    - ip: 192.168.0.20
```
