---
layout: page
title: Основы Kubernets
permalink: /linux/containers/kubernetes/basics/
---

# Основы Kubernets

Replica Sets и Replication Controller считаются устаревшими и рекомендуется вместо них использовать Deployments.

<br/>

### [Kubernetes Pods, Replicasets & Deployments](/linux/containers/kubernetes/basics/pods-replicasets-deployments/)

### [Kubernetes Namespaces & Contexts](/linux/containers/kubernetes/basics/namespaces-and-contexts/)

### [Node Selector in Kubernetes](/linux/containers/kubernetes/basics/node-selector/)

### [Jobs & Cronjobs in Kubernetes](/linux/containers/kubernetes/basics/jobs-and-cronjobs/)

### [Secrets in Kubernetes](/linux/containers/kubernetes/basics/secrets/)

### [ConfigMaps in Kubernetes](/linux/containers/kubernetes/basics/config-maps/)

### [DaemonSets](/linux/containers/kubernetes/basics/daemon-sets/)

### [Init Containers in Kubernetes Cluster](/linux/containers/kubernetes/basics/init-containers/)

### [Resource Quotas & Limits in Kubernetes](/linux/containers/kubernetes/basics/resource-quotas-and-limits/)

### [Services: ClusterIP, NodePort, LoadBalancer, Ingress](/linux/containers/kubernetes/basics/services/)

<br/>

    -- Get the token
    $ TOKEN=$(kubectl describe secret $(kubectl get secrets | grep default | cut -f1 -d ' ') | grep -E '^token' | cut -f2 -d':' | tr -d '\t' | tr -d " ")

    $ echo $TOKEN

    -- get the API server endpoint
    $ APISERVER=$(kubectl config view | grep https | cut -f 2- -d ":" | tr -d " ")


    $ echo $APISERVER

    $ curl $APISERVER --header "Authorization: Bearer $TOKEN" --insecure
