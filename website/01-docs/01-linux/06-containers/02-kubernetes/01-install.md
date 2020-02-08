---
layout: page
title: Инсталляция kubectl ubuntu 18.04
permalink: /linux/containers/kubernetes/install/
---

# Инсталляция kubectl ubuntu 18.04

Делаю:  
10.12.2019

<br/>

VirtualBox должен быть установлен. Используется версия VirtualBox 6.0.4

<br/>

### Инсталляция kubectl (клиента для работы с kubernetes)

<br/>

```shell

-- Текущая стабильная версия kubernetes (v1.17.0)
$ echo $(curl -sS https://storage.googleapis.com/kubernetes-release/release/stable.txt)


-- Установка
$ curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl && chmod +x kubectl && sudo mv kubectl /usr/local/bin/

-- Удалить
$ sudo rm -rf /usr/local/bin/kubectl

```

<br/>

### [Kuberneters на локальном хосте (minikube, kubectl и virtualbox)](/linux/containers/kubernetes/minikube/)

<br/>

### [Vagrant Скрипты разворачивающие Single Master Kubernetes Cluster](/linux/containers/kubernetes/kubeadm/prepared-cluster/)

<br/>

### [Microk8s](/linux/containers/kubernetes/microk8s/)

<br/>

### [Варианты инсталляции kubernetes](/linux/containers/kubernetes/install-types/)
