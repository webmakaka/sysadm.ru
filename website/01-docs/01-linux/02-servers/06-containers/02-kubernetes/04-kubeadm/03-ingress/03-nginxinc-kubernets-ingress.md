---
layout: page
title: Запускаем приложение с использованием NginxInc Kubernetes Ingress
permalink: /linux/servers/containers/kubernetes/kubeadm/ingress/nginxinc-kubernets-ingress/
---

# Запускаем приложение с использованием NginxInc Kubernetes Ingress

Делаю  
04.04.2019

<br/>

По материалам из видео индуса:

https://www.youtube.com/watch?v=YzaYqxW0wGs&list=PL34sAs7_26wNBRWM6BDhnonoA5FMERax0

<br/>

![kubernetes ingress](/img/linux/servers/containers/kubernetes/kubeadm/ingress/ingress.png "kubernetes ingress"){: .center-image }

<br/>
Но вроде должно работать вот так, без всяких haproxy:
<br/>

![kubernetes ingress real](/img/linux/servers/containers/kubernetes/kubeadm/ingress/ingress-real.png "kubernetes ingress real"){: .center-image }

<br/>

Подготовили кластер и окружение как <a href="/linux/servers/containers/kubernetes/kubeadm/prepared-cluster/">здесь</a>.

<br/>

### Добавляю еще 1 виртуалку, на которой будет работать haproxy.

<br/>

    $ mkdir ~/vagrant-kubernetes-haproxy && cd ~/vagrant-kubernetes-haproxy

<br/>

    $ vi Vagrantfile

Виртуалка для NFS

```
# -*- mode: ruby -*-
# vi: set ft=ruby :

ENV['VAGRANT_NO_PARALLEL'] = 'yes'

Vagrant.configure(2) do |config|
  config.vm.box = "centos/7"
  config.hostmanager.enabled = true
  config.hostmanager.include_offline = true

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"
    vb.cpus = 2
  end

  config.vm.define "haproxy.k8s" do |c|
    c.vm.hostname = "haproxy.k8s"
    c.vm.network "private_network", ip: "192.168.0.5"
  end
end
```

<br/>

    $ vagrant up

    $ vagrant ssh haproxy.k8s

<br/>

### Устанавливаем и настраиваем HA proxy

    $ sudo su -

<br/>

    # yum install -y haproxy

<br/>

    # cp /etc/haproxy/haproxy.cfg /etc/haproxy/haproxy.cfg.orig

    # vi /etc/haproxy/haproxy.cfg

<br/>

Оставляем блок global и defaults. Все, что после defaults удаляем.

И добавляем:

```
#---------------------------------------------------------------------
# User defined
#---------------------------------------------------------------------

frontend http_front
  bind *:80
  stats uri /haproxy?stats
  default_backend http_back

backend http_back
  balance roundrobin
  server kube 192.168.0.11:80
  server kube 192.168.0.12:80
```

Предварительно заменив адреса узлов кластера на нужные.

<br/>

    # systemctl enable haproxy
    # systemctl start haproxy
    # systemctl status haproxy

<br/>

### [Создаем NginxInc Kubernetes Ingress контроллер](/linux/servers/containers/kubernetes/kubeadm/ingress/nginxinc-kubernets-ingress-install/)

<br/>

### Запускаем приложение

<br/>

    $ kubectl create -f https://raw.githubusercontent.com/justmeandopensource/kubernetes/master/yamls/ingress-demo/nginx-deploy-blue.yaml

    $ kubectl create -f https://raw.githubusercontent.com/justmeandopensource/kubernetes/master/yamls/ingress-demo/nginx-deploy-green.yaml

    $ kubectl create -f https://raw.githubusercontent.com/justmeandopensource/kubernetes/master/yamls/ingress-demo/ingress-resource-2.yaml

    $ kubectl expose deploy nginx-deploy-blue --port 80

    $ kubectl expose deploy nginx-deploy-green --port 80

    $ kubectl describe ing ingress-resource-2

    $ kubectl get pods
    NAME                                 READY   STATUS    RESTARTS   AGE
    nginx-deploy-blue-7cc7d854dc-9hzwf   1/1     Running   0          3m5s
    nginx-deploy-green-fbbd6d8d8-2nkzb   1/1     Running   0          3m

<br/>

### Настройка на клиенте

<br/>

    # vi /etc/hosts

    192.168.0.5 blue.nginx.example.com
    192.168.0.5 green.nginx.example.com

<br/>

    $ curl blue.nginx.example.com
    <h1>I am <font color=blue>BLUE</font></h1>

    $ curl green.nginx.example.com
    <h1>I am <font color=green>GREEN</font></h1>

<br/>

### Удаляем все это добро:

    $ kubectl delete ing ingress-resource-2

    $ kubectl delete deployment nginx-deploy-blue
    $ kubectl delete deployment nginx-deploy-green

    $ kubectl delete service nginx-deploy-blue
    $ kubectl delete service nginx-deploy-green

<br/>

### Попробуем запустить свое приложение:

    $ mkdir ~/cats-app/ && cd ~/cats-app/

<br/>

    # vi nodejs-cats-app-deployment.yaml

```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: nodejs-cats-app-deployment
spec:
  replicas: 3
  template:
    metadata:
      labels:
        app: nodejs-cats-app
    spec:
      containers:
      - name: nodejs-cats-app
        image: marley/nodejs-cats-app
```

<br/>

    $ vi nodejs-cats-app-svc-nodeport.yaml

```
apiVersion: v1
kind: Service
metadata:
  name: nodejs-cats-app-nodeport
spec:
  type: NodePort
  ports:
  - port: 80
    targetPort: 8080
    nodePort: 30123
  selector:
    app: nodejs-cats-app
```

<br/>

    # vi ingress-resource-nodejs-cats-app.yaml

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: nodejs-cats-app-ingress
spec:
  rules:
  - host: nodejs-cats-app.example.com
    http:
      paths:
      - path: /
        backend:
          serviceName: nodejs-cats-app-nodeport
          servicePort: 80
```

<br/>

    $ kubectl create -f nodejs-cats-app-deployment.yaml
    $ kubectl create -f nodejs-cats-app-svc-nodeport.yaml
    $ kubectl create -f ingress-resource-nodejs-cats-app.yaml

<!--
 # kubectl expose deploy nodejs-cats-app-deployment --port 80

 -->

<br/>

    $ kubectl get pods
    NAME                                          READY   STATUS    RESTARTS   AGE
    nodejs-cats-app-deployment-5d67fbc67d-2bmxz   1/1     Running   0          44m
    nodejs-cats-app-deployment-5d67fbc67d-4jkz2   1/1     Running   0          34m
    nodejs-cats-app-deployment-5d67fbc67d-ghtc5   1/1     Running   0          44m

<br/>

    $ kubectl get deployments
    NAME                         READY   UP-TO-DATE   AVAILABLE   AGE
    nodejs-cats-app-deployment   3/3     3            3           48m

<br/>

    $ kubectl get ing
    NAME                      HOSTS                         ADDRESS   PORTS   AGE
    nodejs-cats-app-ingress   nodejs-cats-app.example.com             80      48m

<br/>

    $ kubectl get service
    NAME                       TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
    kubernetes                 ClusterIP   10.96.0.1        <none>        443/TCP        66m
    nodejs-cats-app-nodeport   NodePort    10.110.183.157   <none>        80:30123/TCP   49m

<br/>

### На клиенте:

    # vi /etc/hosts

    192.168.0.5 nodejs-cats-app.example.com

<br/>

    $ curl nodejs-cats-app.example.com
    OK

<br/>

### Удалить

    # kubectl delete deployment nodejs-cats-app-deployment
    # kubectl delete ing nodejs-cats-app-ingress
    # kubectl delete service nodejs-cats-app-deployment
    # kubectl delete svc nodejs-cats-app-nodeport
