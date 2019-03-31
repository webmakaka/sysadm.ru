---
layout: page
title: Single Master Kubernetes Cluster в виртуальных машинах (vagrant, kubeadm, kubectl) + Traefik ingress
permalink: /linux/servers/containers/kubernetes/kubeadm/single-master-traefik-ingress/
---

# Single Master Kubernetes Cluster в виртуальных машинах (vagrant, kubeadm, kubectl) + Traefik ingress

Делаю  
29.03.2019

По материалам:  
https://www.youtube.com/watch?v=A_PjjCM1eLA&t=2s

Ссылки на сайте Traefik компании не работали!

<br/>

https://docs.traefik.io/v1.6/user-guide/kubernetes/

<br/>

    kubectl apply -f https://raw.githubusercontent.com/containous/traefik/master/examples/k8s/traefik-rbac.yaml

<br/>

    # kubectl describe clusterrole traefik-ingress-controller -n kube-system

    Name:         traefik-ingress-controller
    Labels:       <none>
    Annotations:  kubectl.kubernetes.io/last-applied-configuration:
                    {"apiVersion":"rbac.authorization.k8s.io/v1beta1","kind":"ClusterRole","metadata":{"annotations":{},"name":"traefik-ingress-controller"},"...
    PolicyRule:
    Resources             Non-Resource URLs  Resource Names  Verbs
    ---------             -----------------  --------------  -----
    endpoints             []                 []              [get list watch]
    secrets               []                 []              [get list watch]
    services              []                 []              [get list watch]
    ingresses.extensions  []                 []              [get list watch]

<br/>

    # kubectl get ns
    NAME              STATUS   AGE
    default           Active   21m
    kube-node-lease   Active   21m
    kube-public       Active   21m
    kube-system       Active   21m

<br/>

kubectl apply -f https://raw.githubusercontent.com/containous/traefik/master/examples/k8s/traefik-ds.yaml

    # kubectl get all -n kube-system | grep traefik

    pod/traefik-ingress-controller-9wh84     1/1     Running   0          4m33s
    pod/traefik-ingress-controller-f8ch5     1/1     Running   0          4m33s

    service/traefik-ingress-service   ClusterIP   10.97.47.245   <none>        80/TCP,8080/TCP          4m33s

    daemonset.apps/traefik-ingress-controller   2         2         2       2            2           <none>                            4m33s



    git clone https://github.com/justmeandopensource/kubernetes
    cd /root/kubernetes/yamls/ingress-demo

    # kubectl create -f nginx-deploy-main.yaml -f nginx-deploy-blue.yaml -f nginx-deploy-green.yaml



    # kubectl get svc
    NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
    kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   32m

<br/>

    # kubectl expose deploy nginx-deploy-main --port 80
    # kubectl expose deploy nginx-deploy-blue --port 80
    # kubectl expose deploy nginx-deploy-green --port 80


    # kubectl create -f ingress-resource-2.yaml

    # kubectl get ing
    NAME                 HOSTS                                                              ADDRESS   PORTS   AGE
    ingress-resource-2   nginx.example.com,blue.nginx.example.com,green.nginx.example.com             80      34s



    # kubectl describe ing ingress-resource-2
    Name:             ingress-resource-2
    Namespace:        default
    Address:
    Default backend:  default-http-backend:80 (<none>)
    Rules:
    Host                     Path  Backends
    ----                     ----  --------
    nginx.example.com
                                nginx-deploy-main:80 (10.244.2.4:80)
    blue.nginx.example.com
                                nginx-deploy-blue:80 ()
    green.nginx.example.com
                                nginx-deploy-green:80 (10.244.2.5:80)
    Annotations:
    Events:  <none>

В хостс:

192.168.0.10 nginx.example.com
192.168.0.10 blue.nginx.example.com
192.168.0.10 green.nginx.example.com

    $ curl nginx.example.com
    $ curl blue.nginx.example.com
    $ curl green.nginx.example.com
