---
layout: page
title: Microk8s (в виртуальных машинах)
permalink: /linux/servers/containers/kubernetes/microk8s/
---

# Microk8s (в виртуальных машинах)

Пока непонятно зачем это нужно, кроме как поиграться.
Наверное лучше сразу использовать кластер.

<br/>

По материалам из видео индуса.

https://www.youtube.com/watch?v=YzaYqxW0wGs&list=PL34sAs7_26wNBRWM6BDhnonoA5FMERax0

<br/>

    $ mkdir ~/microk8s && cd ~/microk8s

    # git clone https://github.com/justmeandopensource/vagrant

    $ cd vagrant/vagrantfiles/ubuntu18/

    $ vagrant up

    $ vagrant ssh


    $ sudo apt-get install snapd

    $ sudo snap install microk8s --classic


    $ sudo snap list
    Name      Version  Rev   Tracking  Publisher   Notes
    core      16-2.38  6673  stable    canonical✓  core
    microk8s  v1.14.0  492   stable    canonical✓  classic


    $ microk8s.kubectl cluster-info

    $ microk8s.kubectl version --short
    Client Version: v1.14.0
    Server Version: v1.14.0


    $ alias kubectl='microk8s.kubectl'

    $ kubectl get nodes -o wide
    NAME         STATUS   ROLES    AGE   VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION      CONTAINER-RUNTIME
    ubuntuvm01   Ready    <none>   11m   v1.14.0   10.0.2.15     <none>        Ubuntu 18.04.2 LTS   4.15.0-46-generic   containerd://1.2.5

    $ kubectl run nginx --image nginx
    $ kubectl scale deploy nginx --replicas=2

    $ kubectl expose deploy nginx --port 80 --target-port 80 --type ClusterIP


    $ sudo apt-get update && sudo apt-get install -y elinks

    $ kubectl get all

    // $ elinks < CLUSTER-IP service/nginx >
    $ elinks 10.152.183.53

    $ microk8s.reset

    <br/>

    $ microk8s.enable dashboard