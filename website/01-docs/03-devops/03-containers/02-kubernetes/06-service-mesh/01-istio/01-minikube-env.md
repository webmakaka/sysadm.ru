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
21.01.2021

<br/>

https://istio.io/docs/setup/getting-started/#download

```
$ {
    minikube --profile my-profile config set memory 8192
    minikube --profile my-profile config set cpus 4

    // minikube --profile my-profile config set vm-driver virtualbox
    minikube --profile my-profile config set vm-driver docker

    minikube --profile my-profile config set kubernetes-version v1.20.2
    minikube start --profile my-profile
}
```

<br/>

    // Удалить
    // $ minikube --profile my-profile stop && minikube --profile my-profile delete

<br/>

```
$ kubectl version --short
Client Version: v1.20.2
Server Version: v1.20.2
```

<br/>

### Устанавливаю istioctl на локальном хосте

```
$ curl -L https://istio.io/downloadIstio | sh - && chmod +x $HOME/istio-1.8.2/bin/istioctl && sudo mv $HOME/istio-1.8.2/bin/istioctl /usr/local/bin/
```

<br/>

### Запуск сервисов istio

UPD. Оказазось istio уже есть среди предустановленных расширений на minikube, и можно просто активироваь.

    $ minikube addons --profile my-profile enable istio

Но чего-то ранее не заработало из коробки на 16.9. Не хочу сейчас пробовать.
Поэтому, будем ставить сами.

<br/>

**Дока:**  
https://istio.io/docs/setup/additional-setup/config-profiles/

<br/>

```
$ istioctl profile list
Istio configuration profiles:
    default
    demo
    empty
    minimal
    openshift
    preview
    remote
```

<br/>

```
// $ istioctl manifest install -y --set profile=demo
$ istioctl manifest install -y --set profile=default
```

<br/>

```
$ kubectl label namespace default istio-injection=enabled
```

<br/>

```
$ kubectl describe namespace default
Name:         default
Labels:       istio-injection=enabled
Annotations:  <none>
Status:       Active
```

<br/>

### Добавляю Metal LB

Metal LB позволит получить внешний IP в миникубе на локалхосте.

<br/>

```
$ kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.9.5/manifests/namespace.yaml

$ kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.9.5/manifests/metallb.yaml

# On first install only
$ kubectl create secret generic -n metallb-system memberlist --from-literal=secretkey="$(openssl rand -base64 128)"
```

<br/>

    $ minikube --profile my-profile ip
    192.168.49.2

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
      - 192.168.49.20-192.168.49.30
EOF
```

<br/>

```
$ export INGRESS_HOST=$(kubectl \
 --namespace istio-system \
 get service istio-ingressgateway \
 --output jsonpath="{.status.loadBalancer.ingress[0].ip}")

$ echo ${INGRESS_HOST}
```

<!--

```

$ sudo apt install -y jq

$ kubectl -n istio-system get svc istio-ingressgateway -o json | jq .status.loadBalancer.ingress
[
  {
    "ip": "192.168.49.20"
  }
]
```

-->

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
