---
layout: page
title: Подготовка и установка helm
permalink: /linux/servers/containers/kubernetes/kubeadm/heml/install/
---

# Подготовка и установка helm/tiller

<br/>

Делаю:  
08.10.2019

<br/>

По материалам из видео индуса:

https://www.youtube.com/watch?v=ObGR0EfVPlg&list=PL34sAs7_26wNBRWM6BDhnonoA5FMERax0&index=26

<br/>

Предыдущее видео, в котором он рассказывает о helm обзорное и все повторяется в видео, ссылка на которое выше.

<br/>

    $ kubectl version --short
    Client Version: v1.16.1
    Server Version: v1.16.1


<br/>

### Подготовка

<br/>

Подготовили кластер и окружение как <a href="/linux/servers/containers/kubernetes/kubeadm/prepared-cluster/">здесь</a>.

Подняли Dynamic NFS как<a href="/linux/servers/containers/kubernetes/kubeadm/persistence/dynamic-nfs-provisioning/">здесь</a>.


<br/>

### Инсталляция Helm на хост машине

    // Посмотреть релизы
    https://github.com/helm/helm/releases

    $ curl https://raw.githubusercontent.com/helm/helm/master/scripts/get | bash

    $ helm version --short --client
    Client: v2.14.3+g0e7f3b6

<br/>

### Инсталляция tiller

    $ kubectl -n kube-system create serviceaccount tiller

    $ kubectl create clusterrolebinding tiller --clusterrole cluster-admin --serviceaccount=kube-system:tiller

    $ helm init --service-account tiller

Если ошибка из-за обновившегося Kubernetes (попозже должны будут поправить)

    $HELM_HOME has been configured at /home/marley/.helm.
    Error: error installing: the server could not find the requested resource

Можно попробовать

    $ helm init --service-account tiller --override spec.selector.matchLabels.'name'='tiller',spec.selector.matchLabels.'app'='helm' --output yaml | sed 's@apiVersion: extensions/v1beta1@apiVersion: apps/v1@' | kubectl apply -f -

Продолжаем:

    $ kubectl -n kube-system get pods | grep tiller
    tiller-deploy-8458f6c667-nm6c8       1/1     Running   0          36s
