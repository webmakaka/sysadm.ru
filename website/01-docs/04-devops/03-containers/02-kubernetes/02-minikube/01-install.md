---
layout: page
title: Инсталляция minikube в ubuntu 18.04
permalink: /devops/containers/kubernetes/minikube/install/
---

# Инсталляция minikube в ubuntu 18.04

<br/>

minikube - подготовленная виртуальная машина с мини kubernetes сервером.
Вполне подойдет для изучения kubernetes, особенно на слабых компьютеров и ноутбуках.

<br/>

Делаю:  
22.11.2019

```shell
-- Последняя версия (v1.5.2):
$ curl -s https://api.github.com/repos/kubernetes/minikube/releases/latest | grep '"tag_name":' | sed -E 's/.*"([^"]+)".*/\1/'

-- Установка
$ curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/

```

    $ minikube version
    minikube version: v1.5.2
    commit: 792dbf92a1de583fcee76f8791cff12e0c9440ad-dirty
