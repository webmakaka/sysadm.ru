---
layout: page
title: Single Master Kubernetes Cluster в виртуалках (vagrant, kubeadm, cubectl)
permalink: /linux/servers/containers/kubernetes/kubeadm/single-master/
---

# Single Master Kubernetes Cluster в виртуалках (vagrant, kubeadm, cubectl)

Делаю  
29.03.2019

Нужно поднять 3 виртуальные машины:

<br/>

    192.168.0.10 master.k8s
    192.168.0.11 node1.k8s
    192.168.0.12 node2.k8s

<br/>

Это будет сделано с помощью <a href="/linux/servers/virtual/vagrant/">Vagrant</a>

<br/>

    $ mkdir ~/vagrant-kubernetes && cd ~/vagrant-kubernetes

<br/>

    $ vagrant plugin install vagrant-hostmanager

<br/>

    $ git clone https://bitbucket.org/sysadm-ru/vagrant-kuberntes .

<br/>

    $ vagrant box update
    $ vagrant up

<br/>

    $ vagrant status

    master.k8s running (virtualbox)
    node1.k8s running (virtualbox)
    node2.k8s running (virtualbox)

<br/>

**Устанавливаются следующие пакеты:**

-   docker – контейнерная среда выполнения;
-   kubelet – агент узла Kubernetes, который будет для вас все запускать;
-   kubeadm – инструмент для развертывания многоузловых кластеров
    Kubernetes;
-   kubectl – инструмент командной строки для взаимодействия с
    Kubernetes;
-   kubernetes-cni – интерфейс контейнерного сетевого взаимодействия
    Kubernetes.

<br/>

### Конфигурирование ведущего узла с помощью kubeadm (master.k8s)

    $ vagrant ssh master.k8s

    $ sudo su -

<!--

    // Игнорирую ошибку недостаточности одного CPU и явно указываю интерфейс к которому должны будут подключаться узлы.
    # kubeadm init --ignore-preflight-errors=NumCPU --apiserver-advertise-address 192.168.0.10

-->

    // Явно указываю интерфейс к которому должны будут подключаться узлы.
    # kubeadm init --apiserver-advertise-address 192.168.0.10 --pod-network-cidr=10.244.0.0/16

<br/>

    - apiserver-advertise-address - ip сетевого интерфейса master.k8s к которому должны будут подключаться узлы.
    - pod-network-cidr - какая-то херня, чтобы заработал flanne. Нужно, чтобы контейнеры могли взаимодействовать между собой (если я все правильно понял). Network менять не нужно. Оставить как есть.

<br/>

    // Последние строки в конце скрипта. Их лучше сразу выполнить на добавляемых узлах:

    kubeadm join 192.168.0.10:6443 --token 9v3cmm.7z7r0x2gxwd3kh6g \
    --discovery-token-ca-cert-hash sha256:fb33f100fe8f626a00b65fd8411d2921ac1fff834f3f2c4669305ade67dab74f

<br/>

### Настройка рабочих узлов с помощью kubeadm (node1.k8s, node2.k8s)

    $ vagrant ssh node1.k8s

И сразу:

    $ vagrant ssh node2.k8s

На всех узлах:

    $ sudo su -

Собственно выполняем скрипт, который предлагали на master.k8s

    # kubeadm join 192.168.0.10:6443 --token 9v3cmm.7z7r0x2gxwd3kh6g \
    --discovery-token-ca-cert-hash sha256:fb33f100fe8f626a00b65fd8411d2921ac1fff834f3f2c4669305ade67dab74f

<br/>

### Настройка контейнерной сети (Развертывание плагина Flannel) (master.k8s)

    # export KUBECONFIG=/etc/kubernetes/admin.conf

<br/>

    # kubectl get nodes
    NAME         STATUS     ROLES    AGE   VERSION
    master.k8s   NotReady   master   37s   v1.14.0
    node1.k8s    NotReady   <none>   13s   v1.14.0
    node2.k8s    NotReady   <none>   12s   v1.14.0

<br/>

**Flannel**

Вариант с Weave Net у меня не заработал, но отлично заработал вариант с Flannel.

Узлы уже должны быть подключены к master.k8s.

    # kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/a70459be0084506e4ec919aa1c114638878db11b/Documentation/kube-flannel.yml

<br/>

Ждемс...Пока не будет...

    # kubectl get nodes
    NAME         STATUS   ROLES    AGE   VERSION
    master.k8s   Ready    master   89s   v1.14.0
    node1.k8s    Ready    <none>   65s   v1.14.0
    node2.k8s    Ready    <none>   64s   v1.14.0

<br/>

    # kubectl get po -n kube-system
    NAME                                 READY   STATUS    RESTARTS   AGE
    coredns-fb8b8dccf-9z7b8              1/1     Running   0          87s
    coredns-fb8b8dccf-tsxp9              1/1     Running   0          87s
    etcd-master.k8s                      1/1     Running   0          34s
    kube-apiserver-master.k8s            1/1     Running   0          35s
    kube-controller-manager-master.k8s   1/1     Running   0          45s
    kube-flannel-ds-amd64-2tx79          1/1     Running   0          47s
    kube-flannel-ds-amd64-n9v9f          1/1     Running   0          47s
    kube-flannel-ds-amd64-zqtz8          1/1     Running   0          47s
    kube-proxy-mhwpk                     1/1     Running   0          83s
    kube-proxy-w4snq                     1/1     Running   0          82s
    kube-proxy-wqrjd                     1/1     Running   0          87s
    kube-scheduler-master.k8s            1/1     Running   0          33s

<br/>

    // Если что-то не работает можно попробовать поискать ошибки
    # kubectl describe -n kube-system po coredns-fb8b8dccf-9z7b8

<br/>

    # kubectl get po --all-namespaces

<br/>

### Запуск приложения в созданном кластере

    $ kubectl run nodejs-cats-app --image=marley/nodejs-cats-app --port=8080 --generator=run/v1

<br/>

    $ kubectl get pods
    NAME                       READY   STATUS    RESTARTS   AGE
    nodejs-cats-app-nsk72   1/1     Running   0          23s

<br/>

    $ kubectl expose rc nodejs-cats-app --type=LoadBalancer --name nodejs-cats-app-load-balancer

<br/>

    $ kubectl get services

<br/>

    # kubectl get rc

<br/>

Приложение у меня запустилось:

http://192.168.0.11:30111/

<br/>

### Может быть полезным - литература:

https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/

<br/>

### Использование кластера с локальной машины

    Для этого с помощью следующей ниже команды необходимо скопировать
    файл /etc/kubernetes/admin.conf с ведущего узла на локальную машину:

    $ scp root@192.168.0.10:/etc/kubernetes/admin.conf ~/.kube/config2

<br/>

### Настройки Firewall

**master**

    firewall-cmd --permanent --add-port=6443/tcp
    firewall-cmd --permanent --add-port=2379-2380/tcp
    firewall-cmd --permanent --add-port=10250/tcp
    firewall-cmd --permanent --add-port=10251/tcp
    firewall-cmd --permanent --add-port=10252/tcp
    firewall-cmd --permanent --add-port=10255/tcp
    firewall-cmd --reload
    modprobe br_netfilter

<br/>

**nodes**

    firewall-cmd --permanent --add-port=10250/tcp
    firewall-cmd --permanent --add-port=10255/tcp
    firewall-cmd --permanent --add-port=30000-32767/tcp
    firewall-cmd --permanent --add-port=6783/tcp
    firewall-cmd  --reload
    modprobe br_netfilter
