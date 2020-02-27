---
layout: page
title: Подготовка и установка Helm Tiller
description: Подготовка и установка Helm Tiller
keywords: linux, helm, tiller, install
permalink: /devops/containers/kubernetes/kubeadm/heml/install/
---

# Подготовка и установка Helm/Tiller

<br/>

Делаю:  
22.11.2019

<br/>

По материалам из видео индуса:

https://www.youtube.com/watch?v=ObGR0EfVPlg&list=PL34sAs7_26wNBRWM6BDhnonoA5FMERax0&index=26

<br/>

Предыдущее видео, в котором он рассказывает о helm обзорное и все повторяется в видео, ссылка на которое выше.

<br/>

Обновление после обновления kubernetes до v1.16:

https://www.youtube.com/watch?v=dfQIzPUW8mQ

<br/>

    $ kubectl version --short
    Client Version: v1.16.3
    Server Version: v1.16.3

<br/>

### Подготовка

<br/>

Подготовили кластер и окружение как <a href="/devops/containers/kubernetes/kubeadm/prepared-cluster/">здесь</a>.

Подняли Dynamic NFS как<a href="/devops/containers/kubernetes/kubeadm/persistence/dynamic-nfs-provisioning/">здесь</a>

<br/>

### Инсталляция Helm на хост машине

    // Посмотреть релизы
    https://github.com/helm/helm/releases

<br/>

### Инсталляция версии 3 (Пока юзаю версию 2)

    $ curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash

    $ helm version --short --client
    v3.0.0+ge29ce2a

<br/>

### Инсталляция версии 2

    $ curl https://raw.githubusercontent.com/helm/helm/master/scripts/get | bash

    $ helm version --short --client
    Client: v2.16.3+g1ee0254

<br/>

### Инсталляция tiller

    $ kubectl -n kube-system create serviceaccount tiller

    $ kubectl create clusterrolebinding tiller --clusterrole cluster-admin --serviceaccount=kube-system:tiller

    $ helm init --service-account tiller

    $ kubectl -n kube-system get pods | grep tiller
    tiller-deploy-8458f6c667-nm6c8       1/1     Running   0          36s

<br/>

### Какие-то команды

    $ helm repo update

<br/>

    $ helm search nginx

<br/>

### [Error] Error: error installing: the server could not find the requested resource

Вроде исправили и уже не должна кого-либо беспокоить.

    $ helm init --service-account tiller

Ошибка из-за обновившегося Kubernetes (попозже должны будут поправить)

    $HELM_HOME has been configured at /home/marley/.helm.
    Error: error installing: the server could not find the requested resource

<br/>

    $ helm init --service-account tiller --override spec.selector.matchLabels.'name'='tiller',spec.selector.matchLabels.'app'='helm' --output yaml | sed 's@apiVersion: extensions/v1beta1@apiVersion: apps/v1@' | kubectl apply -f -

    // P.S. можно тоже самое сделать руками
    $ helm init --service-account tiller --output yaml > /tmp/helm.yaml
