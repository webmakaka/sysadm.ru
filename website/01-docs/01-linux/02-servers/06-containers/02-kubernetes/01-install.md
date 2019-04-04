---
layout: page
title: Инсталляция kubectl и minikube в ubuntu 18.04
permalink: /linux/servers/containers/kubernetes/install/
---

# Инсталляция kubectl и minikube

Делаю:  
04.04.2019

<br/>

VirtualBox должен быть установлен. Используется версия VirtualBox 6.0.4

<br/>

### Инсталляция kubectl (клиента для работы с kubernetes)

<br/>

```shell

-- Текущая стабильная версия kubernetes (v1.14.0)
$ echo $(curl -sS https://storage.googleapis.com/kubernetes-release/release/stable.txt)


-- Установка
$ curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl && chmod +x kubectl && sudo mv kubectl /usr/local/bin/

```

<br/>

### Инсталляция minikube (виртуальная машина для изучения и тестов) (Устанавливать при необходимости). Лучше сразу ставить кластер.

```shell
-- Последняя версия (v0.34.1):
$ curl -s https://api.github.com/repos/kubernetes/minikube/releases/latest | grep '"tag_name":' | sed -E 's/.*"([^"]+)".*/\1/'

-- Установка
$ curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/

$ minikube version
minikube version: v0.34.1

```

### [Kuberneters на локальном хосте (minikube, kubectl и virtualbox)](/linux/servers/containers/kubernetes/minikube/)

<br/>

### [Vagrant Скрипты разворачивающие Single Master Kubernetes Cluster](/linux/servers/containers/kubernetes/kubeadm/prepared-cluster/)

<br/>

### [Microk8s](/linux/servers/containers/kubernetes/microk8s/)

<br/>

### Install Helm (что-то вроде package manager). (Устанавливать при необходимости).

    // Посмотреть релизы
    https://github.com/helm/helm/releases

    $ curl https://raw.githubusercontent.com/helm/helm/master/scripts/get | bash

    $ helm version --short --client
    Client: v2.13.1+g618447c

<br/>

### [Варианты инсталляции kubernetes](/linux/servers/containers/kubernetes/install-types/)
