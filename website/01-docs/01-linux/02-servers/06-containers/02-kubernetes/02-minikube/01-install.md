---
layout: page
title: Инсталляция minikube в ubuntu 18.04
permalink: /linux/servers/containers/kubernetes/minikube/install/
---

# Инсталляция minikube в ubuntu 18.04


<br/>

minikube - виртуальная машина для изучения и тестов) (Устанавливать при необходимости). Лучше сразу ставить кластер.

<br/>

Делаю:  
24.10.2019

```shell
-- Последняя версия (v1.4.0):
$ curl -s https://api.github.com/repos/kubernetes/minikube/releases/latest | grep '"tag_name":' | sed -E 's/.*"([^"]+)".*/\1/'

-- Установка
$ curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/

```

    $ minikube version
    minikube version: v1.4.0
    commit: 7969c25a98a018b94ea87d949350f3271e9d64b6