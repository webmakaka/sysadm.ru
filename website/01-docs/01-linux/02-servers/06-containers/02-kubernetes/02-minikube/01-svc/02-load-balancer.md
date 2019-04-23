---
layout: page
title: Создание службы LoadBalancer
permalink: /linux/servers/containers/kubernetes/minikube/svc/load-balancer/
---

# Создание службы LoadBalancer

Делаю:  
02.03.2019

Replica Set созданы как<a href="/linux/servers/containers/kubernetes/minikube/svc/nodeport/">здесь</a>

<br/>

    $ vi nodejs-casts-app-svc-loadbalancer.yaml

```
apiVersion: v1
kind: Service
metadata:
  name: nodejs-casts-app-loadbalancer
spec:
  type: LoadBalancer
  ports:
  - port: 80
    targetPort: 8080
  selector:
    app: nodejs-casts-app
```

<br/>

    $ kubectl create -f nodejs-casts-app-svc-loadbalancer.yaml

<br/>

    $ kubectl get svc
    NAME                              TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
    kubernetes                        ClusterIP      10.96.0.1       <none>        443/TCP        9m15s
    nodejs-casts-app-loadbalancer   LoadBalancer   10.104.16.155   <pending>     80:30340/TCP   11s

<br/>

    $ echo $(minikube service nodejs-casts-app-loadbalancer --url)
    http://192.168.99.105:30340

<br/>

    $ kubectl delete svc nodejs-casts-app-loadbalancer
