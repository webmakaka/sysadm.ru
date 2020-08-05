---
layout: page
title: Инсталляция kubectl ubuntu 18.04
description: Инсталляция kubectl ubuntu 18.04
keywords: linux, kubectl, install
permalink: /devops/containers/kubernetes/install/
---

# Инсталляция kubectl ubuntu 18.04

Делаю:  
27.07.2020

<br/>

### Инсталляция kubectl (клиента для работы с kubernetes)

<br/>

```shell

-- Текущая стабильная версия kubernetes (v1.18.6)
$ echo $(curl -sS https://storage.googleapis.com/kubernetes-release/release/stable.txt)


-- Установка
$ curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl && chmod +x kubectl && sudo mv kubectl /usr/local/bin/

$ kubectl version --client --short
Client Version: v1.18.6


-- Удалить
$ sudo rm -rf /usr/local/bin/kubectl

```

<br/>

### [Kuberneters на локальном хосте (minikube, kubectl и virtualbox)](/devops/containers/kubernetes/minikube/)

<br/>

### [Vagrant Скрипты разворачивающие Single Master Kubernetes Cluster](/devops/containers/kubernetes/kubeadm/vagrant-centos7-3-node-kubernetes-cluster/)

<br/>

### [Microk8s](/devops/containers/kubernetes/microk8s/)

<br/>

### [Варианты инсталляции kubernetes](/devops/containers/kubernetes/install-types/)
