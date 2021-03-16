---
layout: page
title: Инсталляция kubectl ubuntu 20.04
description: Инсталляция kubectl ubuntu 20.04
keywords: devops, linux, kubectl, install
permalink: /devops/containers/kubernetes/kubectl/
---

# Инсталляция kubectl ubuntu 20.04

Делаю:  
10.01.2021

<br/>

### Инсталляция kubectl (клиента для работы с kubernetes)

<br/>

```shell

// Текущая стабильная версия kubernetes (v1.20.1)
$ echo $(curl -sS https://storage.googleapis.com/kubernetes-release/release/stable.txt)


// Установка
$ curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl && chmod +x kubectl && sudo mv kubectl /usr/local/bin/

$ kubectl version --client --short
Client Version: v1.20.1


// Удалить
// $ sudo rm -rf /usr/local/bin/kubectl

```
