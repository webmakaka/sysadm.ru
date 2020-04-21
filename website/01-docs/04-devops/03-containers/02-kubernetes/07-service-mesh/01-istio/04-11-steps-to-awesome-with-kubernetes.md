---
layout: page
title: Istio в minikube
description: Istio в minikube
keywords: linux, kubernetes, Istio, MiniKube
permalink: /devops/containers/kubernetes/service-mesh/istio/minikube/11-steps-to-awesome-with-kubernetes/
---

# Istio в minikube. Примеры из курса "11 Steps to Awesome with Kubernetes, Istio, and Knative LiveLessons"

Делаю:  
01.12.2019

https://github.com/redhat-developer-demos/istio-tutorial

http://github.com/burrsutter/scripts-istio

<br/>

```
$ minikube start \
  -p istio-mk \
  --memory=8192 \
  --cpus=3 \
  --kubernetes-version=v1.14.0 \
  --vm-driver=virtualbox \
  --disk-size=30g
```

<br/>

Копируем:

https://github.com/istio/istio/releases/

<br/>

```
$ export ISTIO_HOME=/home/marley/istio-1.4.0
$ export PATH=$ISTIO_HOME/bin:$PATH
```

<br/>

```
$ cd $ISTIO_HOME

$ for i in install/kubernetes/helm/istio-init/files/crd*yaml; do kubectl apply -f $i; done

$ kubectl apply -f install/kubernetes/istio-demo.yaml

```

<br/>

```
$ kubectl get pods -n istio-system
```

<br/>

### Deploy with Istio Envoy Sidecars

```
$ kubectl create namespace tutorial
$ kubectl config set-context $(kubectl config current-context) --namespace=tutorial
```

<br/>

```
$ mkdir -p ~/tmp/istio && cd ~/tmp/istio

$ git clone https://github.com/redhat-developer-demos/istio-tutorial

$ cd istio-tutorial/

```

<br/>

```

$ istioctl version
client version: 1.4.0
control plane version: 1.4.0
data plane version: 1.4.0 (2 proxies)


$ istioctl kube-inject -f customer/kubernetes/Deployment.yml
$ kubectl label namespace tutorial istio-injection=enabled
```

<br/>

```
$ kubectl get namespaces --show-labels
NAME              STATUS   AGE   LABELS
default           Active   25m   <none>
istio-system      Active   19m   istio-injection=disabled
kube-node-lease   Active   26m   <none>
kube-public       Active   26m   <none>
kube-system       Active   26m   <none>
tutorial          Active   10m   istio-injection=enabled
```

<br/>

```
$ kubectl apply -f customer/kubernetes/Deployment.yml
```

<br/>

```
$ kubectl get pods
NAME                        READY   STATUS    RESTARTS   AGE
customer-5957875cf4-tdcqp   2/2     Running   0          52s
```

<br/>

```
$ kubectl apply -f customer/kubernetes/Service.yml
$ kubectl apply -f customer/kubernetes/Gateway.yml

$ kubectl apply -f preference/kubernetes/Deployment.yml
$ kubectl apply -f preference/kubernetes/Service.yml

$ kubectl apply -f recommendation/kubernetes/Deployment.yml

$ kubectl apply -f recommendation/kubernetes/Service.yml

```

<br/>

```
$ kubectl get services
NAME             TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
customer         ClusterIP   10.107.222.167   <none>        8080/TCP   2m42s
preference       ClusterIP   10.110.74.17     <none>        8080/TCP   73s
recommendation   ClusterIP   10.105.137.121   <none>        8080/TCP   14s

```

<br/>

```
$ kubectl get vs
NAME               GATEWAYS             HOSTS   AGE
customer-gateway   [customer-gateway]   [*]     2m57s
```

<br/>

```

  $ kubectl get services -n istio-system

Видим
istio-ingressgateway -> 31380/TCP


$ minikube --profile istio-mk ip
192.168.99.162

$ curl 192.168.99.162:31380/customer
customer => preference => recommendation v1 from '7d55547f-mg2r8': 205


```

<br/>

```
$ while true; do curl curl 192.168.99.164:31380/customer; sleep .3; done

```

<br/>

### Shift traffic with VirtualService and DestinationRule

https://redhat-developer-demos.github.io/istio-tutorial/istio-tutorial/1.3.x/4simple-routerules.html

```
$ kubectl apply -f recommendation/kubernetes/Deployment-v2.yml

$ curl 192.168.99.164:31380/customer

kubectl scale --replicas=2 deployment/recommendation-v2 -n tutorial

kubectl scale --replicas=1 deployment/recommendation-v2 -n tutorial


kubectl create -f istiofiles/destination-rule-recommendation-v1-v2.yml -n tutorial

kubectl create -f istiofiles/virtual-service-recommendation-v2.yml -n tutorial

```

<br/>

```
$ kubectl get virtualservices
NAME               GATEWAYS             HOSTS              AGE
customer-gateway   [customer-gateway]   [*]                4h29m
recommendation                          [recommendation]   66s
```

```
$ kubectl get destinationrules
NAME             HOST             AGE
recommendation   recommendation   110s


$ kubectl describe vs recommendation

Weight:    100


```

```
$ kubectl edit vs/recommendation

subset: version-v1
```

```
$ while true; do curl curl 192.168.99.164:31380/customer; sleep .3; done

```

```
$ kubectl create -f istiofiles/virtual-service-recommendation-v1_and_v2.yml -n tutorial
```

```
$ kubectl get vs
NAME               GATEWAYS             HOSTS              AGE
customer-gateway   [customer-gateway]   [*]                4h38m
recommendation                          [recommendation]   10m

```

```
$ kubectl delete dr recommendation
$ kubectl delete vs recommendation


$ kubectl delete -f istiofiles/virtual-service-recommendation-v1_and_v2_75_25.yml -n tutorial
$ kubectl delete -f istiofiles/destination-rule-recommendation-v1-v2.yml -n tutorial

```

<br/>

### Perform smarter canary deployments

```
$ kubectl apply -f istiofiles/destination-rule-recommendation-v1-v2.yml -n tutorial

$ kubectl apply -f istiofiles/virtual-service-recommendation-v1_and_v2.yml -n tutorial

$ while true; do curl curl 192.168.99.164:31380/customer; sleep .3; done

$ kubectl edit vs recommendation

60 / 40


$ kubectl delete vs recommendation
$ kubectl delete dr recommendation


<br/>

В зависимости от браузера, региона, залогин пользователь или нет - отдавать контент из определенного сервиса.

```

https://redhat-developer-demos.github.io/istio-tutorial/istio-tutorial/1.3.x/4advanced-routerules.html

<br/>

### Practice mirroring and the dark launch

https://redhat-developer-demos.github.io/istio-tutorial/istio-tutorial/1.3.x/4advanced-routerules.html#mirroringtraffic

```

$ kubectl create -f istiofiles/destination-rule-recommendation-v1-v2.yml -n tutorial

$ kubectl create -f istiofiles/virtual-service-recommendation-v1-mirror-v2.yml -n tutorial

$ while true; do curl curl 192.168.99.164:31380/customer; sleep .3; done

Видим только v1

В общем, если правильно понял. v2 отработает только в случае ошибки.

```

