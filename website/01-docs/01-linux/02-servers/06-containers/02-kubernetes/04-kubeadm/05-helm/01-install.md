---
layout: page
title: Подготовка и установка helm
permalink: /linux/servers/containers/kubernetes/kubeadm/heml/install/
---

# Подготовка и установка helm

<br/>

Делаю: 11.04.2019

<br/>

По материалам из видео индуса:

https://www.youtube.com/watch?v=ObGR0EfVPlg&list=PL34sAs7_26wNBRWM6BDhnonoA5FMERax0&index=26

<br/>

Предыдущее видео, в котором он рассказывает о helm обзорное и все повторяется в видео, ссылка на которое выше.

<br/>

### Подготовка

<br/>

Подготовили кластер и окружение как <a href="/linux/servers/containers/kubernetes/kubeadm/prepared-cluster/">здесь</a>.

Подняли Dynamic NFS как<a href="/linux/servers/containers/kubernetes/kubeadm/persistence/dynamic-nfs-provisioning/">здесь</a>.

<br/>

### Инсталляция

    $ kubectl -n kube-system create serviceaccount tiller

    $ kubectl create clusterrolebinding tiller --clusterrole cluster-admin --serviceaccount=kube-system:tiller

    $ helm init --service-account tiller

    $ kubectl -n kube-system get pods | grep tiller
    tiller-deploy-8458f6c667-nm6c8       1/1     Running   0          36s
