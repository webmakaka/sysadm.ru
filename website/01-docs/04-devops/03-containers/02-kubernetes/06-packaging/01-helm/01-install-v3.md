---
layout: page
title: Подготовка и установка Helm3
description: Подготовка и установка Helm3
keywords: linux, helm3, install
permalink: /devops/containers/kubernetes/packaging/heml3/install/
---

# Подготовка и установка Helm3

<br/>

Делаю:  
23.04.2020


    $ curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash

    $ helm version --short --client
    v3.2.0+ge11b7ce


<br/>

    $ helm repo add stable https://kubernetes-charts.storage.googleapis.com/

    // To remove
    // $ helm repo remove stable

    $ helm repo update


