---
layout: page
title: Introduction to CoreOS Training Video
permalink: /linux/containers/coreos/Introduction_to_CoreOS/
---


# [O’Reilly Media / Infinite Skills] Introduction to CoreOS Training Video [2015, ENG]


https://github.com/rosskukulinski/Introduction_To_CoreOS/

https://github.com/coreos/coreos-vagrant/

<br/>

**Шаги, которые предварительно нужно выполнить:**

    1) Инсталляция Vagrant
    2) Инсталляция VirtualBox
    3) Инсталляция Git


<br/>

**Базовое приложение выглядит следующим образом:**

<br/>

<div align="center">
    <img src="//files.sysadm.ru/img/linux/containers/coreos/app1.png" border="0" alt="cluster">
</div>

<br/>

<div align="center">
    <img src="//files.sysadm.ru/img/linux/containers/coreos/app2.png" border="0" alt="cluster">
</div>

<br/>


<br/>

[01. Launching A Development CoreOS Cluster](/linux/containers/coreos/Introduction_to_CoreOS/Launching_A_Development_CoreOS_Cluster/)

[02. Deploying A DatabaseBacked Web Application](/linux/containers/coreos/Introduction_to_CoreOS/Deploying_A_DatabaseBacked_Web_Application/Deploying_A_DatabaseBacked_Web_Application/)


06\. CoreOS In Production (DiginalOcean, Monitoring, Logging)

07\. Advanced Topics (IPtables, ETCD Security)

08\. Kubernetes (Пока ничего не понятно, или почти ничего)



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
