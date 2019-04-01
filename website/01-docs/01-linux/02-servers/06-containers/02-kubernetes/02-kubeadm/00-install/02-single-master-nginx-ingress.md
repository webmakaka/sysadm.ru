---
layout: page
title: Single Master Kubernetes Cluster в виртуальных машинах (vagrant, kubeadm, kubectl) + nginx ingress
permalink: /linux/servers/containers/kubernetes/kubeadm/install/single-master/nginx-ingress/
---

# Single Master Kubernetes Cluster в виртуальных машинах (vagrant, kubeadm, kubectl) + nginx ingress

// TODO: Нужно переделать

Делаю  
29.03.2019

<br/>

По материалам из видео индуса:

https://www.youtube.com/watch?v=YzaYqxW0wGs&list=PL34sAs7_26wNBRWM6BDhnonoA5FMERax0

<br/>

Нужно поднять 4 виртуальные машины:

<br/>

    192.168.0.10 haproxy.k8s
    192.168.0.11 master.k8s
    192.168.0.12 node1.k8s
    192.168.0.13 node2.k8s

<br/>

Это будет сделано с помощью <a href="/linux/servers/virtual/vagrant/">Vagrant</a>

<br/>
Рисунок индуса:
<br/>

![kubernetes ingress](/img/linux/servers/containers/kubernetes/kubeadm/install/single-master/nginx-ingress/ingress.png "kubernetes ingress"){: .center-image }

<br/>
Но вроде должно работать вот так, без всяких haproxy:
<br/>

![kubernetes ingress real](/img/linux/servers/containers/kubernetes/kubeadm/install/single-master/nginx-ingress/ingress-real.png "kubernetes ingress real"){: .center-image }

<br/>

    $ mkdir ~/vagrant-kubernetes-ingress && cd ~/vagrant-kubernetes-ingress

<br/>

    $ vagrant plugin install vagrant-hostmanager

<br/>

    $ git clone https://bitbucket.org/sysadm-ru/vagrant-kuberntes .
    $ cd centos7/ingress/

<br/>

    (если не будет работать, поправить в скрипте в Vagrant файле до shell скрипта, устанавливающего необходимые пакеты)

<br/>

    $ vagrant box update
    $ vagrant up

<br/>

    $ vagrant status

    haproxy.k8s               running (virtualbox)
    master.k8s                running (virtualbox)
    node1.k8s                 running (virtualbox)
    node2.k8s                 running (virtualbox)

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
    # kubeadm init --apiserver-advertise-address 192.168.0.11 --pod-network-cidr=10.244.0.0/16

<br/>

    - apiserver-advertise-address - ip сетевого интерфейса master.k8s к которому должны будут подключаться узлы.
    - pod-network-cidr - какая-то херня, чтобы заработал flanne. Нужно, чтобы контейнеры могли взаимодействовать между собой (если я все правильно понял). Network менять не нужно. Оставить как есть.

<br/>

    // Последние строки в конце скрипта. Их лучше сразу выполнить на добавляемых узлах:

    kubeadm join 192.168.0.11:6443 --token h8c6u0.eus12it93dk5keix \
    --discovery-token-ca-cert-hash sha256:b582e284bfe43858312c91fe4e86e9e1ad9239e26f21f18d1d44dd2605c9fe60

<br/>

### Настройка рабочих узлов с помощью kubeadm (node1.k8s, node2.k8s)

    $ vagrant ssh node1.k8s

И сразу:

    $ vagrant ssh node2.k8s

На всех узлах:

    $ sudo su -

Собственно выполняем скрипт, который предлагали на master.k8s

    # kubeadm join 192.168.0.11:6443 --token h8c6u0.eus12it93dk5keix \
    --discovery-token-ca-cert-hash sha256:b582e284bfe43858312c91fe4e86e9e1ad9239e26f21f18d1d44dd2605c9fe60

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

и

    # kubectl get nodes
    NAME         STATUS   ROLES    AGE   VERSION
    master.k8s   Ready    master   89s   v1.14.0
    node1.k8s    Ready    <none>   65s   v1.14.0
    node2.k8s    Ready    <none>   64s   v1.14.0

<br/>

    // Если что-то не работает можно попробовать поискать ошибки
    # kubectl describe -n kube-system po coredns-fb8b8dccf-9z7b8

<br/>

    # kubectl get po --all-namespaces

<br/>

### HA proxy

    $ vagrant ssh haproxy.k8s

    $ sudo su -

<br/>

    # yum install -y haproxy

<br/>

    # cp /etc/haproxy/haproxy.cfg /etc/haproxy/haproxy.cfg.orig

    # vi /etc/haproxy/haproxy.cfg

<br/>

Удаляем все после global и defaults.

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
  server kube 192.168.0.12:80
  server kube 192.168.0.13:80
```

Предварительно заменив kube на адреса узлов кластера

<br/>

    # systemctl enable haproxy
    # systemctl start haproxy
    # systemctl status haproxy

<br/>

### Создаем ingress контроллер (master.k8s)

Скрипты:  
https://github.com/nginxinc/kubernetes-ingress

<br/>

    [root@master ~]#

    # kubectl create -f https://raw.githubusercontent.com/nginxinc/kubernetes-ingress/master/deployments/common/ns-and-sa.yaml

<br/>

    # kubectl get ns
    NAME              STATUS   AGE
    default           Active   18m
    kube-node-lease   Active   18m
    kube-public       Active   18m
    kube-system       Active   18m
    nginx-ingress     Active   3s

<br/>

    # kubectl create -f https://raw.githubusercontent.com/nginxinc/kubernetes-ingress/master/deployments/common/default-server-secret.yaml

    # kubectl create -f https://raw.githubusercontent.com/nginxinc/kubernetes-ingress/master/deployments/common/nginx-config.yaml

    # kubectl create -f https://raw.githubusercontent.com/nginxinc/kubernetes-ingress/master/deployments/rbac/rbac.yaml

    # kubectl create -f https://raw.githubusercontent.com/nginxinc/kubernetes-ingress/master/deployments/daemon-set/nginx-ingress.yaml

<br/>

    # kubectl get all -n nginx-ingress
    NAME                      READY   STATUS    RESTARTS   AGE
    pod/nginx-ingress-2hns6   1/1     Running   0          12m
    pod/nginx-ingress-qx6jp   1/1     Running   0          12m

    NAME                           DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
    daemonset.apps/nginx-ingress   2         2         2       2            2           <none>          12m

<br/>

### Запускаем приложение (master.k8s)

<br/>

    # kubectl create -f https://raw.githubusercontent.com/justmeandopensource/kubernetes/master/yamls/ingress-demo/nginx-deploy-blue.yaml

    # kubectl create -f https://raw.githubusercontent.com/justmeandopensource/kubernetes/master/yamls/ingress-demo/nginx-deploy-green.yaml

    # kubectl create -f https://raw.githubusercontent.com/justmeandopensource/kubernetes/master/yamls/ingress-demo/ingress-resource-2.yaml

    # kubectl expose deploy nginx-deploy-blue --port 80

    # kubectl expose deploy nginx-deploy-green --port 80

    # kubectl describe ing ingress-resource-2

<br/>

### Настройка на клиенте

<br/>

    # vi /etc/hosts

    192.168.0.10 blue.nginx.example.com
    192.168.0.10 green.nginx.example.com

<br/>

Через раз работает.

    $ curl blue.nginx.example.com
    <h1>I am <font color=blue>BLUE</font></h1>

    $ curl green.nginx.example.com
    <h1>I am <font color=green>GREEN</font></h1>

<br/>

### Попробуем запустить свое приложение:

    # kubectl delete ing ingress-resource-2

    # kubectl delete deployment nginx-deploy-blue
    # kubectl delete deployment nginx-deploy-green

    # kubectl delete service nginx-deploy-blue
    # kubectl delete service nginx-deploy-green

<br/>

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

    # kubectl create -f nodejs-cats-app-deployment.yaml
    # kubectl create -f nodejs-cats-app-svc-nodeport.yaml
    # kubectl create -f ingress-resource-nodejs-cats-app.yaml

<!--
 # kubectl expose deploy nodejs-cats-app-deployment --port 80

 -->

<br/>

    # kubectl get pods
    NAME                                          READY   STATUS             RESTARTS   AGE
    nodejs-cats-app-deployment-858c47bd58-6868j   1/1     Running            0          8m39s
    nodejs-cats-app-deployment-858c47bd58-vqd4w   1/1     Running            0          8m39s
    nodejs-cats-app-deployment-858c47bd58-wr7j9   0/1     ImagePullBackOff   0          8m39s

<br/>

    # kubectl get deployments
    NAME                         READY   UP-TO-DATE   AVAILABLE   AGE
    nodejs-cats-app-deployment   2/3     3            2           8m21s

<br/>

    # kubectl get ing
    NAME                               HOSTS                         ADDRESS   PORTS   AGE
    ingress-resource-nodejs-cats-app   nodejs-cats-app.example.com             80      6s

<br/>

    # kubectl get service
    NAME                         TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)   AGE
    kubernetes                   ClusterIP   10.96.0.1        <none>        443/TCP   3h3m
    nodejs-cats-app-deployment   ClusterIP   10.108.207.239   <none>        80/TCP    17s

<br/>

### На клиенте:

    # vi /etc/hosts

    192.168.0.10 nodejs-cats-app.example.com

<br/>

    $ curl nodejs-cats-app.example.com

<br/>

### Удалить

    # kubectl delete deployment nodejs-cats-app-deployment
    # kubectl delete ing nodejs-cats-app-ingress
    # kubectl delete service nodejs-cats-app-deployment
    # kubectl delete svc nodejs-cats-app-nodeport

<!--

НЕ ЗАРАБОТАЛО


https://matthewpalmer.net/kubernetes-app-developer/articles/kubernetes-ingress-guide-nginx-example.html

    # kubectl apply -f https://gist.githubusercontent.com/matthewpalmer/d1147ed1066e9219e8c346f4e0dd0488/raw/05b8917f31bd76d9d28fc249d0dfc71523787462/apple.yaml

    # kubectl apply -f https://gist.githubusercontent.com/matthewpalmer/d70ca836c7d7c5da37660923915d9526/raw/242a8261a6acfbc377926d66ffa6e1f995fd251d/banana.yaml

    # kubectl apply -f https://gist.githubusercontent.com/matthewpalmer/9721cf9b3b719bd8ae3af00648cbb484/raw/d703d7330f4bba33c2588230f505e802275e2af9/ingress.yaml

    # kubectl get ing
    NAME              HOSTS   ADDRESS   PORTS   AGE
    example-ingress   *                 80      2m5s

    $ curl -kL http://localhost/apple
    apple

    $ curl -kL http://localhost/banana
    banana


    # kubectl delete ing example-ingress
    # kubectl delete service apple-service
    # kubectl delete service banana-service

-->

<br/>

### Ingress troubleshooting

https://github.com/kubernetes/ingress-nginx/blob/master/docs/troubleshooting.md
