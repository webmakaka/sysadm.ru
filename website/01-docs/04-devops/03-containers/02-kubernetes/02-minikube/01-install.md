---
layout: page
title: Инсталляция minikube в ubuntu 18.04
description: Инсталляция minikube в ubuntu 18.04
keywords: linux, minikube
permalink: /devops/containers/kubernetes/minikube/install/
---

# Инсталляция minikube в ubuntu 18.04

<br/>

minikube - подготовленная виртуальная машина с мини kubernetes сервером.
Вполне подойдет для изучения kubernetes, особенно на слабых компьютеров и ноутбуках.

<br/>

Делаю:  
26.02.2020

```shell
-- Последняя версия (v1.7.3):
$ curl -s https://api.github.com/repos/kubernetes/minikube/releases/latest | grep '"tag_name":' | sed -E 's/.*"([^"]+)".*/\1/'

-- Установка
$ curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/

```

<br/>

```
$ minikube version
minikube version: v1.7.3
commit: 436667c819c324e35d7e839f8116b968a2d0a3ff
```