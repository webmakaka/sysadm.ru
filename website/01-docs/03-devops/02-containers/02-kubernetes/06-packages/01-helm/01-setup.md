---
layout: page
title: Подготовка и установка Helm3
description: Подготовка и установка Helm3
keywords: devops, containers, kubernetes, linux, helm3, setup
permalink: /devops/containers/kubernetes/packes/heml/setup/
---

# Подготовка и установка Helm3

<br/>

Делаю:  
12.01.2021

<br/>

    $ curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash

    $ helm version --short --client
    v3.4.2+g23dd3af

<br/>

    $ helm repo add stable https://charts.helm.sh/stable

    // To remove
    // $ helm repo remove stable

    $ helm repo update
