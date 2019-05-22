---
layout: page
title: Setting up Jenkins on Kubernetes Engine
permalink: /clouds/google/kubernetes/google/jenkins/
---

# [GSP117] Setting up Jenkins on Kubernetes Engine

Делаю!  
22.05.2019

<br/>

https://google.qwiklabs.com/focuses/1776?parent=catalog

<br/>

### Prepare the Environment

    $ gcloud config set compute/zone us-east1-d
    $ git clone https://github.com/GoogleCloudPlatform/continuous-deployment-on-kubernetes.git
    $ cd continuous-deployment-on-kubernetes/

<br/>

### Creating a Kubernetes cluster

    $ gcloud container clusters create jenkins-cd \
    --num-nodes 2 \
    --machine-type n1-standard-2 \
    --scopes "https://www.googleapis.com/auth/projecthosting,cloud-platform"

<br/>

### Install Helm

    $ wget https://storage.googleapis.com/kubernetes-helm/helm-v2.9.1-linux-amd64.tar.gz

    $ tar zxfv helm-v2.9.1-linux-amd64.tar.gz
    $ cp linux-amd64/helm .

    $ kubectl create clusterrolebinding cluster-admin-binding --clusterrole=cluster-admin --user=$(gcloud config get-value account)

    $ kubectl create serviceaccount tiller --namespace kube-system
    
    $ kubectl create clusterrolebinding tiller-admin-binding --clusterrole=cluster-admin --serviceaccount=kube-system:tiller

    $ ./helm init --service-account=tiller
    $ ./helm repo update

    $ ./helm version

<br/>

### Configure and Install Jenkins

    $ ./helm install -n cd stable/jenkins -f jenkins/values.yaml --version 0.16.6 --wait

    // 2 минуты ожидания
    $ kubectl get pods

    $ export POD_NAME=$(kubectl get pods -l "component=cd-jenkins-master" -o jsonpath="{.items[0].metadata.name}")

    $ kubectl port-forward $POD_NAME 8080:8080 >> /dev/null &

<br/>

    $ kubectl get svc                  
    NAME               TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)     AGE
    cd-jenkins         ClusterIP   10.35.251.133   <none>        8080/TCP    5m18s
    cd-jenkins-agent   ClusterIP   10.35.251.188   <none>        50000/TCP   5m18s
    kubernetes         ClusterIP   10.35.240.1     <none>        443/TCP     11m

<br/>

### Connect to Jenkins

    // Получить пароль
    $ printf $(kubectl get secret cd-jenkins -o jsonpath="{.data.jenkins-admin-password}" | base64 --decode);echo


<br/>

web preview --> preview on port 8080

<br/>

Открылось окно. Залогинился.

admin/пароль