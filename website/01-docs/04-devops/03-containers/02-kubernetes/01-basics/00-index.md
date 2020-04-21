---
layout: page
title: Основы Kubernets
permalink: /devops/containers/kubernetes/basics/
---

# Основы Kubernets

Replica Sets и Replication Controller считаются устаревшими и рекомендуется вместо них использовать Deployments.

<br/>

### [Kubernetes Namespaces & Contexts](/devops/containers/kubernetes/basics/namespaces-and-contexts/)

### [Pods](/devops/containers/kubernetes/basics/pods/)

### [ReplicaSets (считается устаревшей. Рекомендуется использовать Deployments)](/devops/containers/kubernetes/basics/replicasets/)

### [Deployments](/devops/containers/kubernetes/basics/deployments/)

### [Services: ClusterIP, NodePort, LoadBalancer, Ingress](/devops/containers/kubernetes/basics/services/)

### [Node Selector in Kubernetes](/devops/containers/kubernetes/basics/node-selector/)

### [Jobs & Cronjobs in Kubernetes](/devops/containers/kubernetes/basics/jobs-and-cronjobs/)

### [Secrets in Kubernetes](/devops/containers/kubernetes/basics/secrets/)

### [ConfigMaps in Kubernetes](/devops/containers/kubernetes/basics/config-maps/)

### [DaemonSets](/devops/containers/kubernetes/basics/daemon-sets/)

### [Init Containers in Kubernetes Cluster](/devops/containers/kubernetes/basics/init-containers/)

### [Resource Quotas & Limits in Kubernetes](/devops/containers/kubernetes/basics/resource-quotas-and-limits/)


<br/>

    -- Get the token
    $ TOKEN=$(kubectl describe secret $(kubectl get secrets | grep default | cut -f1 -d ' ') | grep -E '^token' | cut -f2 -d':' | tr -d '\t' | tr -d " ")

    $ echo $TOKEN

    -- get the API server endpoint
    $ APISERVER=$(kubectl config view | grep https | cut -f 2- -d ":" | tr -d " ")


    $ echo $APISERVER

    $ curl $APISERVER --header "Authorization: Bearer $TOKEN" --insecure
