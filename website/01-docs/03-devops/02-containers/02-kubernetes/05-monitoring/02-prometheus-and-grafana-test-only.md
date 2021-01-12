---
layout: page
title: Запуск Prometheus (мониторинг) и Grafana (визуализация) в kuberntes cluster с помощью heml
description: Запуск Prometheus (мониторинг) и Grafana (визуализация) в kuberntes cluster с помощью heml
keywords: devops, linux, kubernetes, Запуск Prometheus (мониторинг) и Grafana (визуализация) в kuberntes cluster с помощью heml
permalink: /devops/containers/kubernetes/monitoring/prometheus-and-grafana-test-only/
---

# Запуск Prometheus (мониторинг) и Grafana (визуализация) в kuberntes cluster с помощью heml

<br/>

Делаю:  
12.01.2021

<br/>

Запускаю [локальный kubernetes кластер](https://github.com/webmakaka/vagrant-kubernetes-3-node-cluster-centos7)

<br/>

    $ kubectl get nodes
    NAME         STATUS   ROLES                  AGE   VERSION
    master.k8s   Ready    control-plane,master   39h   v1.20.1
    node1.k8s    Ready    <none>                 39h   v1.20.1
    node2.k8s    Ready    <none>                 39h   v1.20.1

<br/>

Устанавливаю [Helm3](/devops/containers/kubernetes/packes/heml/setup/) на localhost.

<br/>

    $ mkdir ~/kubernetes-configs && cd ~/kubernetes-configs

<br/>

### 01. Prometheus

    $ vi prometheus-values.yml

```
alertmanager:
  persistentVolume:
    enabled: false
server:
  persistentVolume:
    enabled: false
```

<br/>

    $ helm search repo prometheus

<br/>

    $ kubectl create namespace prometheus

    $ helm install prometheus \
      -f prometheus-values.yml \
      stable/prometheus \
      --namespace prometheus

    $ kubectl get pods -n prometheus

<br/>

    $ helm list -n prometheus
    NAME      	NAMESPACE 	REVISION	UPDATED                               	STATUS  	CHART             	APP VERSION
    prometheus	prometheus	1       	2021-01-12 15:11:13.92312402 +0300 MSK	deployed	prometheus-11.12.1	2.20.1

<br/>

### 02. Grafana

    $ vi grafana-values.yml

```
adminPassword: password
```

    $ kubectl create namespace grafana

    $ helm install grafana \
      -f grafana-values.yml \
      stable/grafana \
      --namespace grafana

<!--
<br>

    $ vi grafana-ext.yml

```
kind: Service
apiVersion: v1
metadata:
  namespace: grafana
  name: grafana-ext
spec:
  type: NodePort
  selector:
    app: grafana
  ports:
  -   protocol: TCP
      port: 3000
      nodePort: 30001
```

<br/>

    $ kubectl apply -f grafana-ext.yml

<br/>

### Проверка

    $ kubectl get pods -n prometheus
    NAME                                            READY   STATUS    RESTARTS   AGE
    prometheus-alertmanager-5bffbcfdbc-svlrm        2/2     Running   0          7m14s
    prometheus-kube-state-metrics-95d956569-5nqhs   1/1     Running   0          7m14s
    prometheus-node-exporter-227kr                  1/1     Running   0          7m14s
    prometheus-node-exporter-l4x8j                  1/1     Running   0          7m14s
    prometheus-pushgateway-594cd6ff6b-ztww8         1/1     Running   0          7m14s
    prometheus-server-6dc75cbb56-swfl9              2/2     Running   0          7m14s

<br/>

    $ kubectl get pods -n grafana
    NAME                       READY   STATUS    RESTARTS   AGE
    grafana-7f58b98f94-8szjv   1/1     Running   0          2m18s

<br/>

    $ kubectl get svc -n grafana
    NAME          TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
    grafana       ClusterIP   10.102.141.13   <none>        80/TCP           5m2s
    grafana-ext   NodePort    10.105.70.12    <none>        3000:30001/TCP   3m26s

<br/>

http://node1.k8s:30001 -->

<br/>

    $ export POD_NAME=$(kubectl get pods --namespace prometheus -l "app=prometheus,component=server" -o jsonpath="{.items[0].metadata.name}")

    $ kubectl --namespace prometheus port-forward $POD_NAME 9090

http://localhost:9090/graph

<br/>

    $ kubectl port-forward --namespace grafana service/grafana 3000:80

<br/>

http://localhost:3000/

<br/>

Add data source - Prometheus

Name: Prometheus

URL: http://prometheus-server.prometheus.svc.cluster.local

Save and test

<!--

<br/>

    // ISSSUE
    $ export POD_NAME=$(kubectl get pods --namespace grafana -l "app=grafana" -o jsonpath="{.items[0].metadata.name}")


    $ export POD_NAME=grafana-7f58b98f94-8szjv

    $ kubectl --namespace grafana port-forward $POD_NAME 3000

<br/>


https://medium.com/@at_ishikawa/install-prometheus-and-grafana-by-helm-9784c73a3e97

kubectl describe pod grafana-7f58b98f94-8szjv --namespace grafana

-->

<br/>

**Нужно будет Переделать по инструкции:**

https://grafana.com/docs/loki/latest/installation/helm/

<br/>

### Пример запуска приложения с метриками:

<a href="http://itsimple.ru/videos/devops/implementing-a-full-ci-cd-pipeline/monitoring/">Здесь</a>
