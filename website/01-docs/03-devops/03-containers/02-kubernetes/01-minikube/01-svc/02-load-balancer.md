---
layout: page
title: Создание службы LoadBalancer
description: Создание службы LoadBalancers
keywords: devops, containers, kubernetes, minikube, Создание службы LoadBalancers
permalink: /devops/containers/kubernetes/minikube/svc/load-balancer/
---

# Создание службы LoadBalancer

Обновлено:  
12.02.2021

Deployment создан как<a href="/devops/containers/kubernetes/minikube/svc/nodeport/">здесь</a>

<br/>

```yaml
$ cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Service
metadata:
  name: nodejs-casts-app-loadbalancer
spec:
  type: LoadBalancer
  ports:
  - port: 80
    targetPort: 8080
    nodePort: 30123
  selector:
    app: nodejs-cats-app
    env: dev
EOF
```

<br/>

    $ kubectl get svc
    NAME                            TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
    nodejs-casts-app-loadbalancer   LoadBalancer   10.104.115.199   <pending>     80:30123/TCP   55s

<br/>

    $ kubectl describe svc nodejs-casts-app-loadbalancer
    Name:                     nodejs-casts-app-loadbalancer
    Namespace:                demo
    Labels:                   <none>
    Annotations:              Selector:  app=nodejs-cats-app,env=dev
    Type:                     LoadBalancer
    IP:                       10.104.115.199
    Port:                     <unset>  80/TCP
    TargetPort:               8080/TCP
    NodePort:                 <unset>  30123/TCP
    Endpoints:                172.17.0.4:8080,172.17.0.5:8080,172.17.0.6:8080
    Session Affinity:         None
    External Traffic Policy:  Cluster
    Events:                   <none>

<br/>

Чтобы работало, нужно чтобы в Endpoints были перечислены IP виртуальных подов и порты запущенных приложений.

<br/>

    // Если не используется профиль, удалить
    // Если не используется namespace, таке можно убрать -n default
    $ echo $(minikube --profile my-profile service nodejs-casts-app-loadbalancer -n default --url)
    http://192.168.99.113:30123

<br/>

    $ kubectl delete svc nodejs-casts-app-loadbalancer
