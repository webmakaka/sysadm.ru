---
layout: page
title: Создание службы LoadBalancer
permalink: /linux/servers/containers/kubernetes/minikube/svc/load-balancer/
---

# Создание службы LoadBalancer

Делаю:  
02.03.2019

Replica Set созданы как в NodePort

<br/>

    $ vi nodejs-voting-game-svc-loadbalancer.yaml

```
apiVersion: v1
kind: Service
metadata:
  name: nodejs-voting-game-loadbalancer
spec:
  type: LoadBalancer
  ports:
  - port: 80
    targetPort: 8080
  selector:
    app: nodejs-voting-game
```

<br/>

    $ kubectl create -f nodejs-voting-game-svc-loadbalancer.yaml

<br/>

    $ kubectl get svc
    NAME                              TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
    kubernetes                        ClusterIP      10.96.0.1       <none>        443/TCP        9m15s
    nodejs-voting-game-loadbalancer   LoadBalancer   10.104.16.155   <pending>     80:30340/TCP   11s

<br/>

    $ echo $(minikube service nodejs-voting-game-loadbalancer --url)
    http://192.168.99.105:30340

<br/>

    $ kubectl delete svc nodejs-voting-game-loadbalancer
