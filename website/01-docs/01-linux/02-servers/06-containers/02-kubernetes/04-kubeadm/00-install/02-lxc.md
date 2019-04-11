---
layout: page
title: Single Master Kubernetes Cluster в виртуальных lxc контейнерах (kubeadm, kubectl) (Не заработал)
permalink: /linux/servers/containers/kubernetes/kubeadm/install/single-master/lxc/
---

# Single Master Kubernetes Cluster в виртуальных lxc контейнерах (kubeadm, kubectl) (Не заработал)

Делаю  
30.03.2019

(Нужно переделать. Использовать lxc на железке)

<br/>

**По материалам:**

<br/>

<div align="center">
    <iframe width="853" height="480" src="https://www.youtube.com/embed/XQvQUE7tAsk" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>
</div>

<br/>

<br/>

    $ mkdir ~/kubernetes-lxc && cd ~/kubernetes-lxc

<br/>

    $ git clone https://github.com/justmeandopensource/vagrant .

    $ cd vagrant/vagrantfiles/ubuntu18/

<br/>

    $ vi Vagrantfile

    v.memory = 10240
    v.cpus = 4

<br/>

    $ vagrant up
    $ vagrant ssh

<br/>

    $ getent group lxd
    $ sudo gpasswd -a vagrant lxd
    $ id vagrant
    $ logout

<br/>

    $ vagrant ssh

<br/>

    $ sudo systemctl start lxd
    $ sudo systemctl status lxd

<br/>

    // По умолчанию везде, кроме storage backend
    $ lxd init
    Would you like to use LXD clustering? (yes/no) [default=no]:
    Do you want to configure a new storage pool? (yes/no) [default=yes]:
    Name of the new storage pool [default=default]:
    Name of the storage backend to use (btrfs, dir, lvm) [default=btrfs]: [dir]
    Would you like to connect to a MAAS server? (yes/no) [default=no]:
    Would you like to create a new local network bridge? (yes/no) [default=yes]:
    What should the new bridge be called? [default=lxdbr0]:
    What IPv4 address should be used? (CIDR subnet notation, “auto” or “none”) [default=auto]:
    What IPv6 address should be used? (CIDR subnet notation, “auto” or “none”) [default=auto]:
    Would you like LXD to be available over the network? (yes/no) [default=no]:
    Would you like stale cached images to be updated automatically? (yes/no) [default=yes]
    Would you like a YAML "lxd init" preseed to be printed? (yes/no) [default=no]:

<br/>

    $ mkdir ~/kubernetes-lxc && cd ~/kubernetes-lxc
    $ git clone https://github.com/justmeandopensource/kubernetes .
    $ cd lxd-provisioning/

<br/>

    $ lxc profile copy default k8s

<br/>

    $ lxc profile list

<br/>

    $ lxc profile edit k8s

```
config:
  limits.cpu: "2"
  limits.memory: 2GB
  limits.memory.swap: "false"
  linux.kernel_modules: ip_tables,ip6_tables,netlink_diag,nf_nat,overlay
  raw.lxc: "lxc.apparmor.profile=unconfined\nlxc.cap.drop= \nlxc.cgroup.devices.allow=a\nlxc.mount.auto=proc:rw
    sys:rw"
  security.nesting: "true"
  security.privileged: "true"
description: Kubernetes LXD profile
devices:
  eth0:
    name: eth0
    nictype: bridged
    parent: lxdbr0
    type: nic
  root:
    path: /
    pool: default
    type: disk
name: k8s
used_by: []
```

<br/>

    $ lxc profile show k8s

<br/>

    $ lxc launch images:centos/7 kmaster --profile k8s
    $ lxc launch images:centos/7 kworker1 --profile k8s
    $ lxc launch images:centos/7 kworker2 --profile k8s

<br/>

    $ lxc list

<br/>

    $ cd /home/vagrant/kubernetes-lxc/lxd-provisioning

<br/>

    $ cat bootstrap-kube.sh | lxc exec kmaster bash

    $ cat bootstrap-kube.sh | lxc exec kworker1 bash
    $ cat bootstrap-kube.sh | lxc exec kworker2 bash

<br/>

    $ lxc list

<br/>

    $ lxc exec kmaster bash

<br/>

    # kubectl cluster-info
    Kubernetes master is running at https://10.238.168.187:6443
    KubeDNS is running at https://10.238.168.187:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

<br/>

    # kubectl version --short
    Client Version: v1.14.0
    Server Version: v1.14.0

<br/>

    # kubectl get nodes
    NAME       STATUS   ROLES    AGE     VERSION
    kmaster    Ready    master   3m34s   v1.14.0
    kworker1   Ready    <none>   79s     v1.14.0

<br/>

    # kubectl get pods --all-namespaces
    NAMESPACE     NAME                              READY   STATUS    RESTARTS   AGE
    kube-system   coredns-fb8b8dccf-fnch5           1/1     Running   0          4m2s
    kube-system   coredns-fb8b8dccf-zhlqw           1/1     Running   0          4m2s
    kube-system   etcd-kmaster                      1/1     Running   0          2m58s
    kube-system   kube-apiserver-kmaster            1/1     Running   0          3m15s
    kube-system   kube-controller-manager-kmaster   1/1     Running   0          3m12s
    kube-system   kube-flannel-ds-amd64-cv4gp       1/1     Running   0          2m5s
    kube-system   kube-flannel-ds-amd64-j4s4p       1/1     Running   0          4m1s
    kube-system   kube-proxy-dnzsr                  1/1     Running   0          4m1s
    kube-system   kube-proxy-qd5wl                  1/1     Running   0          2m5s
    kube-system   kube-scheduler-kmaster            1/1     Running   0          3m5s

<br/>

    # kubectl get cs
    NAME                 STATUS    MESSAGE             ERROR
    scheduler            Healthy   ok
    controller-manager   Healthy   ok
    etcd-0               Healthy   {"health":"true"}

<br/>

    # kubectl get all -o wide
    NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE     SELECTOR
    service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   5m36s   <none>

<br/>

    # kubectl run nginx --image nginx

<br/>

    # kubectl get all -o wide

    # kubectl describe deploy nginx

    # kubectl scale deploy nginx --replicas 2

    # kubectl delete deploy nginx

<br/>

    # kubectl get nodes
    NAME       STATUS   ROLES    AGE   VERSION
    kmaster    Ready    master   38m   v1.14.0
    kworker1   Ready    <none>   36m   v1.14.0
    kworker2   Ready    <none>   3m    v1.14.0

<br/>

### HA proxy

    $ lxc list

    $ lxc launch images:centos/7 haproxy

    $ lxc list haproxy

    10.238.168.157 (eth0)

<br/>

    $ cd ~/kubernetes-lxc/yamls/ingress-demo/

<br/>

    $ lxc exec haproxy bash

    # yum install -y haproxy

    # vi /etc/haproxy/haproxy.cfg

Удаляем все после global и defaults

И добавляем:

```
frontend http_front
  bind *:80
  stats uri /haproxy?stats
  default_backend http_back

backend http_back
  balance roundrobin
  server kube 10.238.168.17:80
  server kube 10.238.168.28:80
```

    // ip worker с master
    # kubectl get nodes -o wide

<br/>

    # systemctl enable haproxy
    # systemctl start haproxy

<br/>

### Создаем ingress контроллер

[root@kmaster ~]

    # cd ~

    # yum install -y git

    git clone https://github.com/nginxinc/kubernetes-ingress.git

    # cd kubernetes-ingress/deployments

    # kubectl create -f common/ns-and-sa.yaml

    # kubectl get ns
    NAME              STATUS   AGE
    default           Active   73m
    kube-node-lease   Active   73m
    kube-public       Active   73m
    kube-system       Active   73m
    nginx-ingress     Active   16s


    # kubectl create -f common/default-server-secret.yaml

    # kubectl create -f common/nginx-config.yaml

    # kubectl create -f rbac/rbac.yaml

    # kubectl create -f daemon-set/nginx-ingress.yaml

    # kubectl get all -n nginx-ingress

    $ mkdir ~/kubernetes-lxc && cd ~/kubernetes-lxc
    $ git clone https://github.com/justmeandopensource/kubernetes .
    $ cd kubernetes/yamls/ingress-demo/

    # kubectl create -f nginx-deploy-main.yaml

    # kubectl get all

    # kubectl expose deploy nginx-deploy-main --port 80

    # kubectl create -f ingress-resource-1.yaml

    # kubectl get ing
    NAME                 HOSTS               ADDRESS   PORTS   AGE
    ingress-resource-1   nginx.example.com             80      24s

    # kubectl describe ing ingress-resource-1

<br/>

Ничего не заработало, т.к. ingress у меня не видел кластер.

На клиенте в
/etc/hosts

10.238.168.157 nginx.example.com

<br/>

    $ kubectl delete ing ingress-resource-1

    # kubectl create -f nginx-deploy-blue.yaml

    # kubectl create -f nginx-deploy-green.yaml

    # kubectl expose deploy nginx-deploy-blue --port 80

    # kubectl expose deploy nginx-deploy-green --port 80

    # kubectl create -f ingress-resource-2.yaml

    # kubectl describe ing ingress-resource-2

<br/>

На клиенте в
/etc/hosts

10.238.168.157 nginx.example.com
10.238.168.157 blue.nginx.example.com
10.238.168.157 green.nginx.example.com
