---
layout: page
title: Istio
permalink: /linux/servers/containers/kubernetes/kubeadm/istio/
---

# Istio

### НЕ РАБОТАЕТ. НУЖНО ВЕРНУТЬСЯ ПОПОЗЖЕ И ПОПРОБОВАТЬ СНОВА!!!
### ИЛИ ЖДУ ПОДСКАЗОК

Нужно как минимум, чтобы node виртуалок были по 4GB, а не по 2GB, как у меня по умолчанию!

<br/>

Сейчас падают pod с istio-policy и istio-telemetry. Проверка какой-то хрени не проходит. Я не очень понимаю как лечится, да и как это вообще все должно работать.


    $ kubectl describe pod istio-telemetry-646f74c6bf-qzrmb -n istio-system

```
***
Warning  Unhealthy  7m (x42 over 31m)   kubelet, node2.k8s  Liveness probe failed: Get http://10.244.2.5:15014/version: dial tcp 10.244.2.5:15014: connect: connection refused
Warning  BackOff    2m (x123 over 30m)  kubelet, node2.k8s  Back-off restarting failed container
```

    $ kubectl logs istio-telemetry-646f74c6bf-qzrmb  -n istio-system -c mixer

    $ kubectl logs istio-telemetry-646f74c6bf-qzrmb  -n istio-system -c istio-proxy


<br/>

Делаю:  
18.10.2019

<br/>

По материалам из видео индуса:

https://www.youtube.com/watch?v=WFu8OLXUETY&list=PL34sAs7_26wNBRWM6BDhnonoA5FMERax0&index=53

<br/>

    $ kubectl version --short
    Client Version: v1.16.2
    Server Version: v1.16.2


<br/>

### [Устанавливаем Helm и Tiller](/linux/servers/containers/kubernetes/kubeadm/heml/install/)

### [Устанавливаем MetalLB](/linux/servers/containers/kubernetes/kubeadm/metal-load-balancer/)

### Подняли Dynamic NFS как<a href="/linux/servers/containers/kubernetes/kubeadm/persistence/dynamic-nfs-provisioning/">здесь</a>


<br/>

### Инсталляция istioctl (На host машине)

https://istio.io/docs/setup/

    $ curl -L https://git.io/getLatestIstio | ISTIO_VERSION=1.3.3 sh -

<br/>

    $ cd ~/istio-1.3.3

    $ sudo mv bin/istioctl /usr/local/bin

    $ istioctl verify-install


<br/>

### Install with Helm and Tiller via helm install

https://istio.io/docs/setup/install/helm/#option-2-install-with-helm-and-tiller-via-helm-install


    $ pwd
    /home/marley/istio-1.3.3

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
    istio-init-crd-10-1.3.3-fp979   0/1     Completed   0          2m13s
    istio-init-crd-11-1.3.3-65zcs   0/1     Completed   0          2m13s
    istio-init-crd-12-1.3.3-cm9dt   0/1     Completed   0          2m13s


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
    /home/marley/istio-1.3.3

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


