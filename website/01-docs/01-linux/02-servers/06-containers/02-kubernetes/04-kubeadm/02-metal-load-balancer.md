---
layout: page
title: MetalLB Load Balancer in Kubernetes
permalink: /linux/servers/containers/kubernetes/kubeadm/metal-load-balancer/
---

# MetalLB Load Balancer in Kubernetes

Делаю: 05.04.2019

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
      - 192.168.0.20-192.168.0.25
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

    $ kubectl get all
    NAME                          READY   STATUS    RESTARTS   AGE
    pod/nginx-7db9fccd9b-rktn8    1/1     Running   0          6m15s
    pod/nginx2-85bb845f86-2dpfk   1/1     Running   0          31s

    NAME                 TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)        AGE
    service/kubernetes   ClusterIP      10.96.0.1        <none>         443/TCP        30m
    service/nginx        LoadBalancer   10.98.62.210     192.168.0.20   80:30978/TCP   5m59s
    service/nginx2       LoadBalancer   10.106.149.148   192.168.0.21   80:30488/TCP   5m54s

    NAME                     READY   UP-TO-DATE   AVAILABLE   AGE
    deployment.apps/nginx    1/1     1            1           6m15s
    deployment.apps/nginx2   1/1     1            1           6m5s

    NAME                                DESIRED   CURRENT   READY   AGE
    replicaset.apps/nginx-7db9fccd9b    1         1         1       6m15s
    replicaset.apps/nginx2-85bb845f86   1         1         1       6m5s

<br/>

    $ curl 192.168.0.20
    $ curl 192.168.0.21
    OK
