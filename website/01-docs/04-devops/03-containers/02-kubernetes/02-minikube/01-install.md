---
layout: page
title: Инсталляция minikube в ubuntu 18.04
description: Инсталляция minikube в ubuntu 18.04
keywords: linux, minikube
permalink: /devops/containers/kubernetes/minikube/install/
---

# Инсталляция minikube в ubuntu 18.04

<br/>

**minikube** - подготовленная виртуальная машина или контейнер с мини kubernetes сервером.
Вполне подойдет для изучения kubernetes, особенно на слабых компьютерах и ноутбуках.

<br/>

Делаю:  
11.04.2020

```shell
-- Последняя версия (v1.9.2):
$ curl -s https://api.github.com/repos/kubernetes/minikube/releases/latest | grep '"tag_name":' | sed -E 's/.*"([^"]+)".*/\1/'

-- Установка
$ curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/

```

<br/>

```
$ minikube version
minikube version: v1.9.2
```