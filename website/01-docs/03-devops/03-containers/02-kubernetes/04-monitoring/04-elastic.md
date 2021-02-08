---
layout: page
title: Monitor Kubernetes Logs using EFK stack - Elasticsearch, FluentBit & Kibana
description: Monitor Kubernetes Logs using EFK stack - Elasticsearch, FluentBit & Kibana
keywords: devops, linux, kubernetes, Monitor Kubernetes Logs using EFK stack - Elasticsearch, FluentBit & Kibana
permalink: /devops/containers/kubernetes/monitoring/elastic/
---

<br/>

### Elastic Search

```
$ helm repo add elastic https://helm.elastic.co

$ helm repo update

$ kubectl create namespace logging

$ cd ~/tmp

$ curl -O https://raw.githubusercontent.com/elastic/Helm-charts/master/elasticsearch/examples/minikube/values.yaml


$ helm upgrade --install elasticsearch elastic/elasticsearch --namespace logging -f ./values.yaml

```

<br/>

```
$ kubectl port-forward svc/elasticsearch-master 9200 --namespace logging
```

<br/>

### Kibana

```
$ helm upgrade --install kibana elastic/kibana --namespace logging
```

<br/>

```
$ kubectl port-forward deployment/kibana-kibana 5601 --namespace logging
```

<br/>

### Metricbeat

```
$ helm upgrade --install metricbeat elastic/metricbeat --namespace logging
```

<br/>

```
$ kubectl --namespace logging logs elasticsearch-master-0
```

https://logz.io/blog/deploying-the-elk-stack-on-kubernetes-with-helm/

<!--

<br/>

### Fluent-bit

https://docs.fluentbit.io/manual/installation/kubernetes

```

$ helm repo add fluent https://fluent.github.io/helm-charts

$ helm install fluent-bit fluent/fluent-bit


```
-->

<br/>

# Monitor Kubernetes Logs using EFK stack - Elasticsearch, FluentBit & Kibana

<br/>

По материалам из видео индуса:

https://www.youtube.com/watch?v=8nWh1GLd7nY&list=PL34sAs7_26wNBRWM6BDhnonoA5FMERax0&index=35

Используется rancher, все кликается в браузере. Не хочу пока изучать такой материал.
