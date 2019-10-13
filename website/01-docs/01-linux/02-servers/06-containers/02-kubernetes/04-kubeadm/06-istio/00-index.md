---
layout: page
title: Istio
permalink: /linux/servers/containers/kubernetes/kubeadm/istio/
---

# Istio

### НЕ РАБОТАЕТ. НУЖНО ВЕРНУТЬСЯ ПОПОЗЖЕ И ПОПРОБОВАТЬ СНОВА!!!
### ИЛИ ЖДУ ПОДСКАЗОК

<br/>

Делаю:  
08.10.2019

<br/>

По материалам из видео индуса:

https://www.youtube.com/watch?v=WFu8OLXUETY&list=PL34sAs7_26wNBRWM6BDhnonoA5FMERax0&index=53

<br/>

    $ kubectl version --short
    Client Version: v1.16.1
    Server Version: v1.16.1


<br/>

### [Устанавливаем helm и tiller](/linux/servers/containers/kubernetes/kubeadm/heml/install/)

### [Устанавливаем MetalLB](/linux/servers/containers/kubernetes/kubeadm/metal-load-balancer/)


<br/>

### Инсталляция istioctl (На host машине)

https://istio.io/docs/setup/

    $ curl -L https://git.io/getLatestIstio | ISTIO_VERSION=1.3.2 sh -

<br/>

    $ cd istio-1.3.2

    $ sudo mv bin/istioctl /usr/local/bin

    $ istioctl verify-install


<br/>

### Install with Helm and Tiller via helm install

https://istio.io/docs/setup/install/helm/#option-2-install-with-helm-and-tiller-via-helm-install


    $ pwd
    /home/marley/istio-1.3.2

    $ helm install install/kubernetes/helm/istio-init --name istio-init --namespace istio-system

    $ helm status istio-init

    $ watch kubectl -n istio-system get all

<br/>

    $ kubectl get crds | grep istio | wc -l
    23


<br/>

**Была ошибка "istio-pilot 0/3 nodes are available: 1 Insufficient cpu, 3 Insufficient memory"**

Исправляю:

    $ cd /home/marley/istio-1.3.2/install/kubernetes/helm/istio/charts/pilot

    $ vi values.yaml


```
resources:
  requests:
    cpu: 500m
    memory: 2048Mi
```


Указал.

```
resources:
  requests:
    cpu: 100m
    memory: 1048Mi
```

Иначе ошибка

<br/>

    $ kubectl get pods -n istio-system
    NAME                                     READY   STATUS      RESTARTS   AGE
    ***
    istio-pilot-6c7b5ccb46-2p2gb             0/2     Pending     0          36m
    ***

<br/>

    $ helm install install/kubernetes/helm/istio --name istio --namespace istio-system

<br/>

    $ kubectl get pods -n istio-system
    $ kubectl get svc -n istio-system

<br/>

    // Нужно, чтобы MetalLB присвоил ip для LoadBalancer
    $ kubectl get svc -n istio-system | grep LoadBalancer
    istio-ingressgateway     LoadBalancer   10.103.246.44    192.168.0.20   15020:30965/TCP,80:31380/TCP,443:31390/TCP,31400:31400/TCP,15029:30373/TCP,15030:31943/TCP,15031:32735/TCP,15032:31756/TCP,15443:31777/TCP   43m

    

<br/>

### Удаление Istio

    $ helm delete --purge istio
    $ helm delete --purge istio-init


<br/>

### [ Kube 51 ] Istio Deploying sample Bookinfo application (НЕ ЗАРАБОТАЛО!!!)

https://istio.io/docs/examples/bookinfo/


    $ kubectl label namespace default istio-injection=enabled

    $ kubectl get ns --show-labels
    NAME              STATUS   AGE    LABELS
    default           Active   123m   istio-injection=enabled
    istio-system      Active   55m    name=istio-system
    kube-node-lease   Active   123m   <none>
    kube-public       Active   123m   <none>
    kube-system       Active   123m   <none>
    metallb-system    Active   30m    app=metallb

<br/>


    $ pwd
    /home/marley/istio-1.3.2

<br/>

    $ kubectl create -f samples/bookinfo/platform/kube/bookinfo.yaml

    $ kubectl get services

    $ kubectl get pods

    $ kubectl get all

<br/>

    $ kubectl get deployments
    NAME             READY   UP-TO-DATE   AVAILABLE   AGE
    details-v1       0/1     0            0           16m
    productpage-v1   0/1     0            0           16m
    ratings-v1       0/1     0            0           16m
    reviews-v1       0/1     0            0           16m
    reviews-v2       0/1     0            0           16m
    reviews-v3       0/1     0            0           16m

<br/>

    $ kubectl describe deployments productpage-v1

<br/>

    $ kubectl get events -w

<br/>

    $ kubectl create -f samples/bookinfo/networking/bookinfo-gateway.yaml
    $ kubectl get gateway
    $ kubectl describe gateway bookinfo-gateway


<br/>

    $ kubectl -n istio-system get svc istio-ingressgateway

<br/>

http://192.168.0.20/productpage