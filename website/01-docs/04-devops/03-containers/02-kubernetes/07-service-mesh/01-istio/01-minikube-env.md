---
layout: page
title: Подготовка окружения для тестов Istio в minikube
description: Подготовка окружения для тестов Istio в minikube
keywords: linux, kubernetes, Istio, MiniKube
permalink: /devops/containers/kubernetes/service-mesh/istio/minikube/env/
---

# Подготовка окружения для тестов Istio в minikube

<br/>

Делаю:  
18.04.2020

<br/>

https://istio.io/docs/setup/getting-started/#download


```
$ {
minikube --profile my-profile config set memory 4096
minikube --profile my-profile config set cpus 2

minikube --profile my-profile config set vm-driver virtualbox
// minikube --profile my-profile config set vm-driver docker

minikube --profile my-profile config set kubernetes-version v1.16.1
minikube start --profile my-profile
}
```

<br/>

    // Удалить
    // $ minikube --profile my-profile stop && minikube --profile my-profile delete

<br/>

    $ kubectl version --short
    Client Version: v1.18.1
    Server Version: v1.16.1



<br/>

### Устанавливаю Istio на локальном хосте

    $ curl -L https://istio.io/downloadIstio | sh - && chmod +x $HOME/istio-1.5.1/bin/istioctl && sudo mv $HOME/istio-1.5.1/bin/istioctl /usr/local/bin/

<br/>

    $ istioctl manifest apply --set profile=demo

<br/>

    $ kubectl label namespace default istio-injection=enabled


<br/>

### Всевозможные проверки

    $ watch kubectl -n istio-system get all

<br/>

    // Жду пока не появится 25
    $ kubectl get crds | grep istio | wc -l
    25

<br/>

    $ kubectl get pods -n istio-system
    NAME                                    READY   STATUS    RESTARTS   AGE
    grafana-5cc7f86765-8qkg2                1/1     Running   0          79s
    istio-egressgateway-598d7ffc49-h64bn    1/1     Running   0          80s
    istio-ingressgateway-7bd5586b79-t8khf   1/1     Running   0          80s
    istio-tracing-8584b4d7f9-qs2hc          1/1     Running   0          78s
    istiod-646b6fcc6-55dhd                  1/1     Running   0          108s
    kiali-696bb665-f9sqh                    1/1     Running   0          78s
    prometheus-6c88c4cb8-b294t              2/2     Running   0          78s

<br/>

    $ kubectl get svc -n istio-system



<br/>

### Добавляю Metal LB

Metal LB позволит получить внешний IP в миникубе на локалхосте

<br/>

    $ kubectl apply -f https://raw.githubusercontent.com/google/metallb/v0.8.3/manifests/metallb.yaml

<br/>

    $ minikube --profile my-profile ip
    192.168.99.105

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
    - name: custom-ip-space
      protocol: layer2
      addresses:
      - 192.168.99.105/28
EOF
```
