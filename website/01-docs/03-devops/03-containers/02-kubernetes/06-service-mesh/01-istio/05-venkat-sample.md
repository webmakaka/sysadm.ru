---
layout: page
title: Istio в локальном kubernetes кластере
description: Istio в локальном kubernetes кластере
keywords: linux, kubernetes, Istio
permalink: /devops/containers/kubernetes/service-mesh/istio/venkat-sample/
---

# Istio в локальном kubernetes кластере

<br/>

Делаю:  
22.11.2019

<br/>

По материалам из видео индуса Венката:

https://www.youtube.com/watch?v=WFu8OLXUETY&list=PL34sAs7_26wNBRWM6BDhnonoA5FMERax0&index=53

<br/>

**Нужно как минимум, чтобы node виртуалок были по 4GB, а не по 2GB, как у меня по умолчанию!**

<br/>

    $ kubectl version --short
    Client Version: v1.16.3
    Server Version: v1.16.3

<br/>

### [Helm](/devops/containers/kubernetes/packes/heml/setup/)

### [Устанавливаем MetalLB](/devops/containers/kubernetes/metal-lb/)

### Подняли Dynamic NFS как<a href="/devops/containers/kubernetes/kubeadm/persistence/dynamic-nfs-provisioning/">здесь</a>

<br/>

### Инсталляция istioctl (На host машине)

https://istio.io/docs/setup/

    $ curl -L https://git.io/getLatestIstio | ISTIO_VERSION=1.4.0 sh -

<br/>

    $ cd ~/istio-1.4.0

    $ sudo mv bin/istioctl /usr/local/bin

    $ istioctl verify-install

<br/>

### Install with Helm and Tiller via helm install

https://istio.io/docs/setup/install/helm/#option-2-install-with-helm-and-tiller-via-helm-install

    $ pwd
    /home/marley/istio-1.4.0

    $ helm install install/kubernetes/helm/istio-init --name istio-init --namespace istio-system

    $ helm status istio-init

    $ watch kubectl -n istio-system get all

<br/>

    // Жду пока не появится 23
    $ kubectl get crds | grep istio | wc -l
    23

<br/>

    $ kubectl get pods -n istio-system
    NAME                            READY   STATUS      RESTARTS   AGE
    istio-init-crd-10-1.4.0-2fq5s   0/1     Completed   0          45s
    istio-init-crd-11-1.4.0-jr8xq   0/1     Completed   0          45s
    istio-init-crd-14-1.4.0-7b428   0/1     Completed   0          45s

<br/>

    $ helm install install/kubernetes/helm/istio --name istio --namespace istio-system

<br/>

    $ kubectl get svc -n istio-system

<br/>

    $ kubectl get pods -n istio-system
    NAME                                      READY   STATUS             RESTARTS   AGE
    istio-citadel-67f6594c46-g59xf            1/1     Running            0          14m
    istio-galley-6c7fcf86d4-82hk9             1/1     Running            0          14m
    istio-ingressgateway-6d68548679-tkshm     0/1     Running            0          14m
    istio-init-crd-10-1.3.3-97brz             0/1     Completed          0          14m
    istio-init-crd-11-1.3.3-p8pw2             0/1     Completed          0          14m
    istio-init-crd-12-1.3.3-qj7lp             0/1     Completed          0          14m
    istio-pilot-5cd79c98b9-gck5l              1/2     Running            0          14m
    istio-policy-59d8f8c9f8-2p6jz             1/2     CrashLoopBackOff   9          14m
    istio-sidecar-injector-6d967869b5-qs6vl   1/1     Running            0          14m
    istio-telemetry-646f74c6bf-qzrmb          1/2     CrashLoopBackOff   9          14m
    prometheus-6f74d6f76d-n5dfm               1/1     Running            0          14m

<br/>

    // Нужно, чтобы MetalLB присвоил ip для LoadBalancer
    $ kubectl get svc -n istio-system | grep LoadBalancer
    istio-ingressgateway     LoadBalancer   10.103.246.44    192.168.0.20   15020:30965/TCP,80:31380/TCP,443:31390/TCP,31400:31400/TCP,15029:30373/TCP,15030:31943/TCP,15031:32735/TCP,15032:31756/TCP,15443:31777/TCP   43m

<br/>

### Если понадобится удалить Istio

    $ helm delete --purge istio
    $ helm delete --purge istio-init

<br/>

### [ Kube 51 ] Istio Deploying sample Bookinfo application

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
    /home/marley/istio-1.4.0

<br/>

    $ kubectl create -f samples/bookinfo/platform/kube/bookinfo.yaml

    $ kubectl get services

    $  kubectl get pods
    NAME                                     READY   STATUS    RESTARTS   AGE
    details-v1-78d78fbddf-d9wkr              2/2     Running   0          2m17s
    nfs-client-provisioner-b48654857-6j2tz   1/1     Running   0          10m
    productpage-v1-596598f447-fm4dt          2/2     Running   0          2m17s
    ratings-v1-6c9dbf6b45-pmckx              2/2     Running   0          2m17s
    reviews-v1-7bb8ffd9b6-dh8b5              2/2     Running   0          2m17s
    reviews-v2-d7d75fff8-cszfw               2/2     Running   0          2m18s
    reviews-v3-68964bc4c8-rx4qc              2/2     Running   0          2m18s

<br/>

    $ kubectl get deployments
    NAME                     READY   UP-TO-DATE   AVAILABLE   AGE
    details-v1               1/1     1            1           2m36s
    nfs-client-provisioner   1/1     1            1           11m
    productpage-v1           1/1     1            1           2m36s
    ratings-v1               1/1     1            1           2m36s
    reviews-v1               1/1     1            1           2m36s
    reviews-v2               1/1     1            1           2m36s
    reviews-v3               1/1     1            1           2m36s

<!--
<br/>

    $ kubectl describe deployments productpage-v1

<br/>

    $ kubectl get events -w

<br/>

-->

    $ kubectl create -f samples/bookinfo/networking/bookinfo-gateway.yaml
    $ kubectl get gateway
    $ kubectl describe gateway bookinfo-gateway

<br/>

    $ kubectl -n istio-system get svc istio-ingressgateway
    NAME                   TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)                                                                                                                                      AGE
    istio-ingressgateway   LoadBalancer   10.102.200.254   192.168.0.21   15020:30509/TCP,80:31380/TCP,443:31390/TCP,31400:31400/TCP,15029:31067/TCP,15030:30711/TCP,15031:30863/TCP,15032:32006/TCP,15443:31001/TCP   7m10s

<br/>

http://192.168.0.21/productpage

<br/>

![Istio](/img/devops/containers/kubernetes/service-mesh/istio/pic-01.png 'Istio'){: .center-image }

<!--

<br/>

### Ошибки istio-pilot

Нужно установить 4GB оперативной памяти и все будет ок.

<br/>

    $ kubectl describe pod istio-pilot-789d4748b-glttm -n istio-system

<br/>

    Events:
    Type     Reason            Age        From               Message
    ----     ------            ----       ----               -------
    Warning  FailedScheduling  <unknown>  default-scheduler  0/3 nodes are available: 1 Insufficient cpu, 3 Insufficient memory.
    Warning  FailedScheduling  <unknown>  default-scheduler  0/3 nodes are available: 1 Insufficient cpu, 3 Insufficient memory.

<br/>

Исправляю:

    $ cd /home/marley/istio-1.3.3/install/kubernetes/helm/istio/charts/pilot

    $ cp values.yaml values.yaml.orig

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
    cpu: 400m
    memory: 1500Mi
```

Не помогло.

<br/>

    $ helm delete --purge istio
    $ helm delete --purge istio-init

<br/>

    $ cd ~/istio-1.3.3/

    $ helm install install/kubernetes/helm/istio-init --name istio-init --namespace istio-system

    $ helm install install/kubernetes/helm/istio --name istio --namespace istio-system

    $ kubectl get pods -n istio-system


-->
