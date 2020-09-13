---
layout: page
title: Инсталляция minikube в ubuntu 20.04.1
description: Инсталляция minikube в ubuntu 20.04.1
keywords: linux, minikube
permalink: /devops/containers/kubernetes/minikube/install/
---

# Инсталляция minikube в ubuntu 20.04.1

<br/>

**minikube** - подготовленная виртуальная машина или контейнер с мини kubernetes сервером.
Вполне подойдет для изучения kubernetes, особенно на слабых компьютерах и ноутбуках.

<br/>

Делаю:  
13.09.2020

```shell
-- Последняя версия (v1.13.0):
$ curl -s https://api.github.com/repos/kubernetes/minikube/releases/latest | grep '"tag_name":' | sed -E 's/.*"([^"]+)".*/\1/'

-- Установка
$ curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/

```

<br/>

```
$ minikube version
minikube version: v1.13.0
```