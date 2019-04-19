---
layout: page
title: Устанавливаем и настраиваем HAProxy для Kuberntes (тестовые задачи)
permalink: /linux/servers/containers/kubernetes/kubeadm/ingress/haproxy/
---

# Устанавливаем и настраиваем HAProxy для Kuberntes (тестовые задачи)

Делаю  
19.04.2019

<br/>

По материалам из видео индуса:

https://www.youtube.com/watch?v=YzaYqxW0wGs&list=PL34sAs7_26wNBRWM6BDhnonoA5FMERax0

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
