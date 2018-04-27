---
layout: page
title: Kubernetes > Первое знакомство
permalink: /linux/containers/kubernetes/first-look/
---


# Kubernetes > Первое знакомство

<br/>

### Используемые материалы

<br/>

<div align="center">

    <iframe width="640" height="360" src="https://www.youtube.com/embed/9uDivcfCdUA" frameborder="0" allowfullscreen></iframe>

</div>

<br/>

По материалам:  
http://containertutorials.com/get_started_kubernetes/index.html

<br/>
<hr/>
<br/>

### Подготовка окружения к инсталляции kubernetes

Я работаю на ubuntu 14.04.  
В версии 16.04 это работать не будет.
Systemd и все такое. Точнее, для 16.04 предется делать как-то по-другому.


<br/>

    $  lsb_release -a
    No LSB modules are available.
    Distributor ID:	Ubuntu
    Description:	Ubuntu 14.04.5 LTS
    Release:	14.04
    Codename:	trusty

<br/>

    $ sudo su -

    # apt-get update && apt-get install -y ssh curl links traceroute docker.io python

<br/>

Генерируем ключи и обеспечиваем доступ по ssh без пароля.

    # ssh-keygen -t rsa

[Enter]

     # cat /root/.ssh/id_rsa.pub >> /root/.ssh/authorized_keys

<br/>

Проверка:

     # ssh root@127.0.0.1


<br/>

### Установка kubernetes

    # mkdir -p /opt/kubernetes/1.0.1

    # cd /tmp/

    # wget https://github.com/GoogleCloudPlatform/kubernetes/releases/download/v1.0.1/kubernetes.tar.gz


<br/>

    # tar -xvf kubernetes.tar.gz
    # cd kubernetes/
    # mv * /opt/kubernetes/1.0.1/

<br/>

    # cd /opt/kubernetes/1.0.1/cluster/ubuntu/
    # ./build.sh

<br/>

    # ls binaries
    kubectl  master  minion

<br/>

    # cp config-default.sh config-default.sh.orig

<br/>

    # vi config-default.sh

заменить параметры на следующие:

    export nodes="root@127.0.0.1"
    export roles="ai"
    export NUM_MINIONS=${NUM_MINIONS:-1}


<br/>

    # export PATH=$PATH:/opt/bin

    # cd /opt/kubernetes/1.0.1/cluster/

    # KUBERNETES_PROVIDER=ubuntu ./kube-up.sh


<br/>


    $ export PATH=$PATH:/opt/kubernetes/1.0.1/cluster/ubuntu/binaries/

    # kubectl get nodes
    NAME        LABELS                             STATUS
    127.0.0.1   kubernetes.io/hostname=127.0.0.1   Ready


<br/>

    # cd /opt/kubernetes/1.0.1/cluster/
    # kubectl create -f addons/kube-ui/kube-ui-rc.yaml --namespace=kube-system
    # kubectl create -f addons/kube-ui/kube-ui-svc.yaml --namespace=kube-system


<br/>

### Create a Wordpress pod example

<br/>

    # kubectl run wordpress --image=tutum/wordpress --port=80 --hostport=81
    CONTROLLER   CONTAINER(S)   IMAGE(S)          SELECTOR        REPLICAS
    wordpress    wordpress      tutum/wordpress   run=wordpress   1

<br/>

    # kubectl get pods
    NAME              READY     REASON    RESTARTS   AGE
    wordpress-iqi15   1/1       Running   0          57s


<br/>

    // replication controller

    # kubectl get rc
    CONTROLLER   CONTAINER(S)   IMAGE(S)          SELECTOR        REPLICAS
    wordpress    wordpress      tutum/wordpress   run=wordpress   1


<br/>

    # docker ps
    CONTAINER ID        IMAGE                                   COMMAND             CREATED              STATUS              PORTS                NAMES
    aea3791b7953        tutum/wordpress:latest                  "/run.sh"           48 seconds ago       Up 48 seconds                            k8s_wordpress.b1cc1d5b_wordpress-iqi15_default_a383079a-c4a6-11e6-b9e8-0800277e8445_5e539479      
    017422b8b8a3        gcr.io/google_containers/pause:0.8.0    "/pause"            About a minute ago   Up About a minute   0.0.0.0:81->80/tcp   k8s_POD.9a88e88a_wordpress-iqi15_default_a383079a-c4a6-11e6-b9e8-0800277e8445_bbca3123            
    cd2046d54887        gcr.io/google_containers/kube-ui:v1.1   "/kube-ui"          2 minutes ago        Up 2 minutes                             k8s_kube-ui.301383b8_kube-ui-v1-r9jk8_kube-system_6d42b567-c4a6-11e6-b9e8-0800277e8445_5e4dfa46   
    d5954eb132de        gcr.io/google_containers/pause:0.8.0    "/pause"            3 minutes ago        Up 3 minutes                             k8s_POD.3b46e8b9_kube-ui-v1-r9jk8_kube-system_6d42b567-c4a6-11e6-b9e8-0800277e8445_e19896e6

<br/>

    # links 127.0.0.1:81

<br/>

### Deleting the Kubernetes Cluster

    # KUBERNETES_PROVIDER=ubuntu ./kube-down.sh
