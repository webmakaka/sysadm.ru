---
layout: page
title: Подготовка окружения для тестов Istio в minikube
description: Подготовка окружения для тестов Istio в minikube
keywords: devops, containers, kubernetes, service-mesh, istio, minikube, setup
permalink: /devops/containers/kubernetes/service-mesh/istio/minikube/setup/
---

# Подготовка окружения для тестов Istio в minikube

<br/>

Делаю:  
12.02.2021

<br/>

https://istio.io/docs/setup/getting-started/#download

```
$ {
    minikube --profile istio-tests config set memory 8192
    minikube --profile istio-tests config set cpus 4

    // minikube --profile istio-tests config set vm-driver virtualbox
    minikube --profile istio-tests config set vm-driver docker

    minikube --profile istio-tests config set kubernetes-version v1.20.2
    minikube start --profile istio-tests --embed-certs
}
```

<br/>

    // Удалить
    // $ minikube --profile istio-tests stop && minikube --profile istio-tests delete

<br/>

```
$ kubectl version --short
Client Version: v1.20.2
Server Version: v1.20.2
```

<br/>

### Устанавливаю istioctl на локальном хосте

```
$ cd ~/tmp/
$ curl -L https://istio.io/downloadIstio | sh - && chmod +x ./istio-1.9.0/bin/istioctl && sudo mv ./istio-1.9.0/bin/istioctl /usr/local/bin/
```

<br/>

### Запуск сервисов istio

UPD. Оказазось istio уже есть среди предустановленных расширений на minikube, и можно просто активироваь.

    $ minikube addons --profile istio-tests enable istio

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

После этого, **новые** создаваемые pod будут "проксируемыми". Т.е. старые нужно пересоздать.

<br/>

```
$ kubectl get ns --show-labels
NAME              STATUS   AGE   LABELS
default           Active   26m   istio-injection=enabled
istio-system      Active   19m   istio-injection=disabled
kube-node-lease   Active   26m   <none>
kube-public       Active   26m   <none>
kube-system       Active   26m   <none>
metallb-system    Active   12m   app=metallb
```

<br/>

### Добавляю Metal LB

Metal LB позволит получить внешний IP в миникубе на локалхосте.

<br/>

```
$ LATEST_VERSION=$(curl --silent "https://api.github.com/repos/metallb/metallb/releases/latest" | grep '"tag_name"' | sed -E 's/.*"([^"]+)".*/\1/')

$ echo ${LATEST_VERSION}
```

<br/>

```
$ kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/${LATEST_VERSION}/manifests/namespace.yaml

$ kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/${LATEST_VERSION}/manifests/metallb.yaml

# On first install only
$ kubectl create secret generic -n metallb-system memberlist --from-literal=secretkey="$(openssl rand -base64 128)"
```

<br/>

```
$ minikube --profile istio-tests ip
192.168.49.2
```

<br/>

Задаем диапазон ip адресов, которые можно выдать виртуальному сервису. Нужно, чтобы он был в той же подсети, что и ip minikube.

<br/>

```yaml
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

<br/>

### Всевозможные проверки

    $ watch kubectl -n istio-system get all

<br/>

    // 12
    $ kubectl get crds | grep istio | wc -l
    12

<br/>

```
$ kubectl get pods -n istio-system
NAME                                    READY   STATUS    RESTARTS   AGE
istio-ingressgateway-758985db4f-d2jdh   1/1     Running   0          2m
istiod-7c9c9d46d4-rh7fz                 1/1     Running   0          2m18s

```

<br/>

```
$ kubectl get svc -n istio-system
NAME                   TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)                                                                      AGE
istio-ingressgateway   LoadBalancer   10.106.144.8   192.168.49.20   15021:32220/TCP,80:32270/TCP,443:31357/TCP,15012:30385/TCP,15443:30134/TCP   3m27s
istiod                 ClusterIP      10.99.91.237   <none>          15010/TCP,15012/TCP,443/TCP,15014/TCP                                        3m45s
```

<br/>

### Дополнительные сервисы (Prometheus, Grafana, Kiali, Jaeger):

```
$ cd ~/tmp/istio-1.9.0/samples/addons/
$ kubectl apply -n istio-system -f ./
```

<br/>

Чтобы запустился Kiali нужно повторить

```
$ kubectl apply -n istio-system -f ./kiali.yaml
```

<br/>

```
$ kubectl -n istio-system get pods
NAME                                    READY   STATUS    RESTARTS   AGE
grafana-784c89f4cf-zv7rd                1/1     Running   0          2m40s
istio-ingressgateway-758985db4f-d2jdh   1/1     Running   0          11m
istiod-7c9c9d46d4-rh7fz                 1/1     Running   0          11m
jaeger-7f78b6fb65-rwjcx                 1/1     Running   0          87s
kiali-dc84967d9-xndpn                   1/1     Running   0          87s
prometheus-7bfddb8dbf-bnwcf             2/2     Running   0          87s
```

<br/>

```
$ kubectl -n istio-system port-forward svc/grafana 3000
$ kubectl -n istio-system port-forward svc/prometheus 9090
$ kubectl -n istio-system port-forward svc/kiali 20001
```

<br/>

Пока непонятно как работать с jaeger

```
$ kubectl -n istio-system port-forward svc/jaeger-collector 14268
```

<br/>

### Zipkin и Prometheus Operator

```
$ cd ~/tmp/istio-1.9.0/samples/addons/extras
$ kubectl apply -n istio-system -f ./
```

<br/>

```
$ kubectl apply -n istio-system -f ./prometheus-operator.yaml
unable to recognize "./prometheus-operator.yaml": no matches for kind "PodMonitor" in version "monitoring.coreos.com/v1"
unable to recognize "./prometheus-operator.yaml": no matches for kind "ServiceMonitor" in version "monitoring.coreos.com/v1"
```

<br/>

В общем, нужно еще и добавлять из стандартной установки компоненты, чтобы он понимал, что за ServiceMonitor и PodMonitor.

Пока неактуально.
