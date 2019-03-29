---
layout: page
title: Инсталляция kubectl и minikube в ubuntu 18.04
permalink: /linux/servers/containers/kubernetes/minikube/cubect-minikube-installation/
---

# Инсталляция kubectl и minikube

Делаю:  
28.02.2019

<br/>

VirtualBox должен быть установлен. Используется версия VirtualBox 6.0.4

<br/>

### Install kubectl

<br/>

```shell

-- Текущая стабильная версия kubernetes (v1.13.3)
$ echo $(curl -sS https://storage.googleapis.com/kubernetes-release/release/stable.txt)


-- Установка
$ curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl && chmod +x kubectl && sudo mv kubectl /usr/local/bin/

```

<br/>

### Install minikube

```shell
-- Последняя версия (v0.34.1):
$ curl -s https://api.github.com/repos/kubernetes/minikube/releases/latest | grep '"tag_name":' | sed -E 's/.*"([^"]+)".*/\1/'

-- Установка
$ curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/

$ minikube version
minikube version: v0.34.1

```
