---
layout: page
title: Single Master Kubernetes Cluster в lxc контейнерах
description: Single Master Kubernetes Cluster в lxc контейнерах
keywords: devops, linux, kubernetes,  Single Master Kubernetes Cluster в lxc контейнерах
permalink: /devops/containers/kubernetes/kubeadm/install/single-master/lxc/
---

# Single Master Kubernetes Cluster в lxc контейнерах

Делаю  
12.04.2019

<br/>

**По материалам:**

<br/>

<div align="center">
    <iframe width="853" height="480" src="https://www.youtube.com/embed/XQvQUE7tAsk" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>
</div>

<br/>

### Приступим

Инсталлировали и сделали Init как <a href="/devops/containers/lxc/ubuntu/">здесь</a>

<br/>

    $ mkdir ~/kubernetes-lxc && cd ~/kubernetes-lxc
    $ git clone https://bitbucket.org/sysadm-ru/kubernetes .
    $ cd lxd-provisioning/

<br/>

    $ lxc profile copy default k8s

<br/>

    $ lxc profile list
    +---------+---------+
    |  NAME   | USED BY |
    +---------+---------+
    | default | 0       |
    +---------+---------+
    | k8s     | 0       |
    +---------+---------+

<br/>

    $ export EDITOR=vim
    $ lxc profile edit k8s

Сетевой адаптер eth0 менять не понадобилось, хотся на хостовой машине и нет адаптера с именем eth0.

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
    +----------+---------+----------------------+------+------------+-----------+
    |   NAME   |  STATE  |         IPV4         | IPV6 |    TYPE    | SNAPSHOTS |
    +----------+---------+----------------------+------+------------+-----------+
    | kmaster  | RUNNING | 10.81.125.137 (eth0) |      | PERSISTENT | 0         |
    +----------+---------+----------------------+------+------------+-----------+
    | kworker1 | RUNNING | 10.81.125.61 (eth0)  |      | PERSISTENT | 0         |
    +----------+---------+----------------------+------+------------+-----------+
    | kworker2 | RUNNING | 10.81.125.179 (eth0) |      | PERSISTENT | 0         |
    +----------+---------+----------------------+------+------------+-----------+

<br/>

    $ cat bootstrap-kube.sh | lxc exec kmaster bash

    $ cat bootstrap-kube.sh | lxc exec kworker1 bash
    $ cat bootstrap-kube.sh | lxc exec kworker2 bash

<br/>

    $ lxc list
    +----------+---------+------------------------+------+------------+-----------+
    |   NAME   |  STATE  |          IPV4          | IPV6 |    TYPE    | SNAPSHOTS |
    +----------+---------+------------------------+------+------------+-----------+
    | kmaster  | RUNNING | 172.17.0.1 (docker0)   |      | PERSISTENT | 0         |
    |          |         | 10.81.125.137 (eth0)   |      |            |           |
    |          |         | 10.244.0.1 (cni0)      |      |            |           |
    |          |         | 10.244.0.0 (flannel.1) |      |            |           |
    +----------+---------+------------------------+------+------------+-----------+
    | kworker1 | RUNNING | 172.17.0.1 (docker0)   |      | PERSISTENT | 0         |
    |          |         | 10.81.125.61 (eth0)    |      |            |           |
    |          |         | 10.244.1.0 (flannel.1) |      |            |           |
    +----------+---------+------------------------+------+------------+-----------+
    | kworker2 | RUNNING | 172.17.0.1 (docker0)   |      | PERSISTENT | 0         |
    |          |         | 10.81.125.179 (eth0)   |      |            |           |
    +----------+---------+------------------------+------+------------+-----------+

<br/>

    $ lxc exec kmaster bash

<br/>

    # kubectl version --short
    Client Version: v1.14.1
    Server Version: v1.14.1

<br/>

    # kubectl get nodes
    NAME       STATUS   ROLES    AGE     VERSION
    kmaster    Ready    master   6m21s   v1.14.1
    kworker1   Ready    <none>   3m19s   v1.14.1
    kworker2   Ready    <none>   63s     v1.14.1

<br/>

### Останавливаем и удаляем lxc конейнеры

    $ lxc stop kmaster && lxc delete kmaster
    $ lxc stop kworker1 && lxc delete kworker1
    $ lxc stop kworker2 && lxc delete kworker2
