---
layout: page
title: Инсталляция cubectl и minikube
permalink: /linux/servers/containers/kubernetes/localhost/cubect-minikube-installation/
---

# Инсталляция cubectl и minikube

<br/>


VirtualBox должен быть установлен


<br/>

### Install kubectl


<br/>

```shell

-- Текущая стабильная версия kubernetes
$ echo $(curl -sS https://storage.googleapis.com/kubernetes-release/release/stable.txt)
v1.13.1

-- Установка
$ curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl && chmod +x kubectl && sudo mv kubectl /usr/local/bin/

```

<!--

Было. Удалить попозже!!!

$ curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl

$ chmod +x ./kubectl
$ sudo mv ./kubectl /usr/local/bin/kubectl

-->
    

<br/>

### Install minikube

```shell
-- Последняя версия:
$ curl -s https://api.github.com/repos/kubernetes/minikube/releases/latest | grep '"tag_name":' | sed -E 's/.*"([^"]+)".*/\1/'

-- Установка
$ curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/

$ minikube version
minikube version: v0.31.0

```
    
 