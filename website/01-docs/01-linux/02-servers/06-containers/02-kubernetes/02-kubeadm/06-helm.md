---
layout: page
title: Менеджер пакетов helm (начинаем разбираться)
permalink: /linux/servers/containers/kubernetes/kubeadm/heml/
---

# Менеджер пакетов helm (начинаем разбираться)

<br/>

По материалам из видео индуса:

https://www.youtube.com/watch?v=HTj3MMZE6zg&index=25&list=PL34sAs7_26wNBRWM6BDhnonoA5FMERax0

<br/>

    // Установка
    https://sysadm.ru/linux/servers/containers/kubernetes/install/

    $ kubectl -n kube-system create serviceaccount tiller

    $ kubectl create clusterrolebinding tiller --clusterrole cluster-admin --serviceaccount=kube-system:tiller

    $ kubectl get clusterrolebinding tiller
    NAME     AGE
    tiller   21s

    $ helm init --service-account tiller

    $ kubectl -n kube-system get pods | grep tiller
    tiller-deploy-8458f6c667-rz588                1/1     Running   0          50s

    $ helm home
    /home/marley/.helm


    $ helm repo listNAME  	URL
    stable	https://kubernetes-charts.storage.googleapis.com
    local 	http://127.0.0.1:8879/charts

    $ helm search jenkins

    $ helm inspect jenkins

    $ helm inspect stable/jenkins

    $ helm fetch stable/jenkins

    $ tar zxf jenkins-0.36.2.tgz


    $ helm reset --remove-helm-home
