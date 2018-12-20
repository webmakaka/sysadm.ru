---
layout: page
title: Kubernetes
permalink: /linux/servers/containers/coreos/introduction-to-coreos/deploying-a-atabase-backed-web-application/kubernetes/
---


# [O’Reilly Media / Infinite Skills] Introduction to CoreOS Training Video [2015, ENG] : Kubernetes



1) Создали 1 coreOS с конфигом из master.yml и 2 с конфигом из minion.yml.  
В minion.yml заменили IP адрес.

    ./kubectl get nodes
    ./kubectl get services

    cluster-info


    ./kubectl create -f pod-nginx.yml

    ./kubectl get pods

    ./kubectl describe pod nginx

    ./kubectl logs nginx nginx

    fleetctl --tunnel=IP list-machines

    ./kubectl delete pod nginx

    curl IP:80

    ===================

    Create replication controller

    ./kubectl create -f nginx-rc.yml

    ./kubectl get replicatincontrollers


    kubectl describe replicatincontrollers nginx-rc


    ./kubectl get pods
