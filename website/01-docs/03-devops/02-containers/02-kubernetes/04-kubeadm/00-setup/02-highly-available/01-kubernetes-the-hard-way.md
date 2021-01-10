---
layout: page
title: Kubernetes The Hard Way
description: Kubernetes The Hard Way
keywords: devops, linux, kubernetes,   Kubernetes The Hard Way
permalink: /devops/containers/kubernetes/kubeadm/setup/highly-available/kubernetes-the-hard-way/
---

# Kubernetes The Hard Way

https://github.com/kelseyhightower/kubernetes-the-hard-way

Индус записал видео по настройке того же самого или приблизительно того же самоно но на своих железках а не в облаках.

https://www.youtube.com/watch?v=NvQY5tuxALY

<br/>

Делаю  
28.05.2019

<br/>

![Kubernetes The Hard Way](/img/devops/containers/kubernetes/kubeadm/setup/highly-available/kubernetes-the-hard-way/pic01.png 'Kubernetes The Hard Way'){: .center-image }

![Kubernetes The Hard Way](/img/devops/containers/kubernetes/kubeadm/setup/highly-available/kubernetes-the-hard-way/pic02.png 'Kubernetes The Hard Way'){: .center-image }

![Kubernetes The Hard Way](/img/devops/containers/kubernetes/kubeadm/setup/highly-available/kubernetes-the-hard-way/pic03.png 'Kubernetes The Hard Way'){: .center-image }

![Kubernetes The Hard Way](/img/devops/containers/kubernetes/kubeadm/setup/highly-available/kubernetes-the-hard-way/pic04.png 'Kubernetes The Hard Way'){: .center-image }

![Kubernetes The Hard Way](/img/devops/containers/kubernetes/kubeadm/setup/highly-available/kubernetes-the-hard-way/pic05.png 'Kubernetes The Hard Way'){: .center-image }

![Kubernetes The Hard Way](/img/devops/containers/kubernetes/kubeadm/setup/highly-available/kubernetes-the-hard-way/pic06.png 'Kubernetes The Hard Way'){: .center-image }

![Kubernetes The Hard Way](/img/devops/containers/kubernetes/kubeadm/setup/highly-available/kubernetes-the-hard-way/pic07.png 'Kubernetes The Hard Way'){: .center-image }

![Kubernetes The Hard Way](/img/devops/containers/kubernetes/kubeadm/setup/highly-available/kubernetes-the-hard-way/pic08.png 'Kubernetes The Hard Way'){: .center-image }

![Kubernetes The Hard Way](/img/devops/containers/kubernetes/kubeadm/setup/highly-available/kubernetes-the-hard-way/pic09.png 'Kubernetes The Hard Way'){: .center-image }

![Kubernetes The Hard Way](/img/devops/containers/kubernetes/kubeadm/setup/highly-available/kubernetes-the-hard-way/pic10.png 'Kubernetes The Hard Way'){: .center-image }

<br/>

    $ lxc launch images:centos/7 haproxy

    $ lxc launch ubuntu:18.04 controller-0 --profile k8s
    $ lxc launch ubuntu:18.04 controller-1 --profile k8s
    $ lxc launch ubuntu:18.04 controller-2 --profile k8s


    $ lxc launch ubuntu:18.04 worker-0 --profile k8s
    $ lxc launch ubuntu:18.04 worker-1 --profile k8s
    $ lxc launch ubuntu:18.04 worker-2 --profile k8s

    $ lxc list
    +--------------+---------+----------------------+------+------------+-----------+
    |     NAME     |  STATE  |         IPV4         | IPV6 |    TYPE    | SNAPSHOTS |
    +--------------+---------+----------------------+------+------------+-----------+
    | controller-0 | RUNNING | 10.81.125.195 (eth0) |      | PERSISTENT | 0         |
    +--------------+---------+----------------------+------+------------+-----------+
    | controller-1 | RUNNING | 10.81.125.253 (eth0) |      | PERSISTENT | 0         |
    +--------------+---------+----------------------+------+------------+-----------+
    | controller-2 | RUNNING | 10.81.125.234 (eth0) |      | PERSISTENT | 0         |
    +--------------+---------+----------------------+------+------------+-----------+
    | haproxy      | RUNNING | 10.81.125.83 (eth0)  |      | PERSISTENT | 0         |
    +--------------+---------+----------------------+------+------------+-----------+
    | worker-0     | RUNNING | 10.81.125.9 (eth0)   |      | PERSISTENT | 0         |
    +--------------+---------+----------------------+------+------------+-----------+
    | worker-1     | RUNNING | 10.81.125.231 (eth0) |      | PERSISTENT | 0         |
    +--------------+---------+----------------------+------+------------+-----------+
    | worker-2     | RUNNING | 10.81.125.10 (eth0)  |      | PERSISTENT | 0         |
    +--------------+---------+----------------------+------+------------+-----------+

<br/>

### HAProxy

    $ lxc exec haproxy bash
    # yum install -y haproxy net-tools

    # vi /etc/haproxy/haproxy.cfg

Оставляем блок global и defaults. Все, что после defaults удаляем.

И добавляем:

```
#---------------------------------------------------------------------
# User defined
#---------------------------------------------------------------------

frontend k8s
    bind 10.81.125.83:6443
    mode tcp
    default_backend k8s

backend k8s
    balance roundrobin
    mode tcp
    option tcplog
    option tcp-check
    server controler-0 10.81.125.195:6443 check
    server controler-1 10.81.125.253:6443 check
    server controler-2 10.81.125.234:6443 check
```

<br/>

    # systemctl enable haproxy
    # systemctl start haproxy
    # systemctl status haproxy

    # netstat -nltp

<br/>

### На хост машине

https://github.com/kelseyhightower/kubernetes-the-hard-way/blob/master/docs/02-client-tools.md

<br/>

    $ wget -q --show-progress --https-only --timestamping \
      https://pkg.cfssl.org/R1.2/cfssl_linux-amd64 \
      https://pkg.cfssl.org/R1.2/cfssljson_linux-amd64

<br/>

    $ chmod +x cfssl_linux-amd64 cfssljson_linux-amd64

    $ sudo mv cfssl_linux-amd64 /usr/local/bin/cfssl

    $ sudo mv cfssljson_linux-amd64 /usr/local/bin/cfssljson

    $ cfssl version
    Version: 1.2.0
    Revision: dev
    Runtime: go1.6

kubectl уже инсталлирован.

    $ kubectl version --short --client
    Client Version: v1.14.1

<br/>

### 02 Generatin TLS Certificates

https://github.com/kelseyhightower/kubernetes-the-hard-way/blob/master/docs/04-certificate-authority.md

<br/>

    $ mkdir play
    $ cd play

<br/>

**Certificate Authority**

```
{

cat > ca-config.json <<EOF
{
  "signing": {
    "default": {
      "expiry": "8760h"
    },
    "profiles": {
      "kubernetes": {
        "usages": ["signing", "key encipherment", "server auth", "client auth"],
        "expiry": "8760h"
      }
    }
  }
}
EOF

cat > ca-csr.json <<EOF
{
  "CN": "Kubernetes",
  "key": {
    "algo": "rsa",
    "size": 2048
  },
  "names": [
    {
      "C": "US",
      "L": "Portland",
      "O": "Kubernetes",
      "OU": "CA",
      "ST": "Oregon"
    }
  ]
}
EOF

cfssl gencert -initca ca-csr.json | cfssljson -bare ca

}

```

<br/>

**The Admin Client Certificate**

```
{

cat > admin-csr.json <<EOF
{
  "CN": "admin",
  "key": {
    "algo": "rsa",
    "size": 2048
  },
  "names": [
    {
      "C": "US",
      "L": "Portland",
      "O": "system:masters",
      "OU": "Kubernetes The Hard Way",
      "ST": "Oregon"
    }
  ]
}
EOF

cfssl gencert \
  -ca=ca.pem \
  -ca-key=ca-key.pem \
  -config=ca-config.json \
  -profile=kubernetes \
  admin-csr.json | cfssljson -bare admin

}

```

<br/>

**The Kubelet Client Certificates**

меняем оригинальный файл.

```
for instance in worker-0 worker-1 worker-2; do
cat > ${instance}-csr.json <<EOF
{
  "CN": "system:node:${instance}",
  "key": {
    "algo": "rsa",
    "size": 2048
  },
  "names": [
    {
      "C": "US",
      "L": "Portland",
      "O": "system:nodes",
      "OU": "Kubernetes The Hard Way",
      "ST": "Oregon"
    }
  ]
}
EOF

EXTERNAL_IP=$(lxc info ${instance} | grep eth0 | head -1 | awk '{print $3}')

cfssl gencert \
  -ca=ca.pem \
  -ca-key=ca-key.pem \
  -config=ca-config.json \
  -hostname=${instance},${EXTERNAL_IP} \
  -profile=kubernetes \
  ${instance}-csr.json | cfssljson -bare ${instance}
done
```

<br/>

    $ ls -l worker*pem
    -rw------- 1 marley marley 1675 May 28 02:42 worker-0-key.pem
    -rw-r--r-- 1 marley marley 1484 May 28 02:42 worker-0.pem
    -rw------- 1 marley marley 1679 May 28 02:42 worker-1-key.pem
    -rw-r--r-- 1 marley marley 1484 May 28 02:42 worker-1.pem
    -rw------- 1 marley marley 1679 May 28 02:42 worker-2-key.pem
    -rw-r--r-- 1 marley marley 1484 May 28 02:42 worker-2.pem

<br/>

**The Controller Manager Client Certificate**

```
{

cat > kube-controller-manager-csr.json <<EOF
{
  "CN": "system:kube-controller-manager",
  "key": {
    "algo": "rsa",
    "size": 2048
  },
  "names": [
    {
      "C": "US",
      "L": "Portland",
      "O": "system:kube-controller-manager",
      "OU": "Kubernetes The Hard Way",
      "ST": "Oregon"
    }
  ]
}
EOF

cfssl gencert \
  -ca=ca.pem \
  -ca-key=ca-key.pem \
  -config=ca-config.json \
  -profile=kubernetes \
  kube-controller-manager-csr.json | cfssljson -bare kube-controller-manager

}

```

<br/>

**The Kube Proxy Client Certificate**

```
{

cat > kube-proxy-csr.json <<EOF
{
  "CN": "system:kube-proxy",
  "key": {
    "algo": "rsa",
    "size": 2048
  },
  "names": [
    {
      "C": "US",
      "L": "Portland",
      "O": "system:node-proxier",
      "OU": "Kubernetes The Hard Way",
      "ST": "Oregon"
    }
  ]
}
EOF

cfssl gencert \
  -ca=ca.pem \
  -ca-key=ca-key.pem \
  -config=ca-config.json \
  -profile=kubernetes \
  kube-proxy-csr.json | cfssljson -bare kube-proxy

}

```

**The Scheduler Client Certificate**

```
{

cat > kube-scheduler-csr.json <<EOF
{
  "CN": "system:kube-scheduler",
  "key": {
    "algo": "rsa",
    "size": 2048
  },
  "names": [
    {
      "C": "US",
      "L": "Portland",
      "O": "system:kube-scheduler",
      "OU": "Kubernetes The Hard Way",
      "ST": "Oregon"
    }
  ]
}
EOF

cfssl gencert \
  -ca=ca.pem \
  -ca-key=ca-key.pem \
  -config=ca-config.json \
  -profile=kubernetes \
  kube-scheduler-csr.json | cfssljson -bare kube-scheduler

}
```

<br/>

**The Kubernetes API Server Certificate**

Меняем оригинальный конфиг.

    KUBERNETES_PUBLIC_ADDRESS - ip адрес haproxy
    hostname - ip адреса контроллеров

```
{

KUBERNETES_PUBLIC_ADDRESS=10.81.125.83

cat > kubernetes-csr.json <<EOF
{
  "CN": "kubernetes",
  "key": {
    "algo": "rsa",
    "size": 2048
  },
  "names": [
    {
      "C": "US",
      "L": "Portland",
      "O": "Kubernetes",
      "OU": "Kubernetes The Hard Way",
      "ST": "Oregon"
    }
  ]
}
EOF

cfssl gencert \
  -ca=ca.pem \
  -ca-key=ca-key.pem \
  -config=ca-config.json \
  -hostname=10.81.125.195,10.81.125.253,10.81.125.234,${KUBERNETES_PUBLIC_ADDRESS},127.0.0.1,kubernetes.default \
  -profile=kubernetes \
  kubernetes-csr.json | cfssljson -bare kubernetes

}
```

<br/>

**The Service Account Key Pair**

```
{

cat > service-account-csr.json <<EOF
{
  "CN": "service-accounts",
  "key": {
    "algo": "rsa",
    "size": 2048
  },
  "names": [
    {
      "C": "US",
      "L": "Portland",
      "O": "Kubernetes",
      "OU": "Kubernetes The Hard Way",
      "ST": "Oregon"
    }
  ]
}
EOF

cfssl gencert \
  -ca=ca.pem \
  -ca-key=ca-key.pem \
  -config=ca-config.json \
  -profile=kubernetes \
  service-account-csr.json | cfssljson -bare service-account

}
```

<br/>

    $ ls
    admin.csr                         kube-proxy-csr.json       service-account.pem
    admin-csr.json                    kube-proxy-key.pem        worker-0.csr
    admin-key.pem                     kube-proxy.pem            worker-0-csr.json
    admin.pem                         kubernetes.csr            worker-0-key.pem
    ca-config.json                    kubernetes-csr.json       worker-0.pem
    ca.csr                            kubernetes-key.pem        worker-1.csr
    ca-csr.json                       kubernetes.pem            worker-1-csr.json
    ca-key.pem                        kube-scheduler.csr        worker-1-key.pem
    ca.pem                            kube-scheduler-csr.json   worker-1.pem
    kube-controller-manager.csr       kube-scheduler-key.pem    worker-2.csr
    kube-controller-manager-csr.json  kube-scheduler.pem        worker-2-csr.json
    kube-controller-manager-key.pem   service-account.csr       worker-2-key.pem
    kube-controller-manager.pem       service-account-csr.json  worker-2.pem
    kube-proxy.csr                    service-account-key.pem

<br/>

**Distribute the Client and Server Certificates**

    $ for instance in worker-0 worker-1 worker-2; do
      lxc file push ca.pem ${instance}-key.pem ${instance}.pem ${instance}/root/
    done

<br/>

    $ for instance in controller-0 controller-1 controller-2; do
      lxc file push ca.pem ca-key.pem kubernetes-key.pem kubernetes.pem \
        service-account-key.pem service-account.pem ${instance}/root/
    done

<br/>

    $ lxc exec worker-0 ls
    ca.pem	worker-0-key.pem  worker-0.pem

<br/>

    $ lxc exec controller-0 ls
    ca-key.pem  kubernetes-key.pem	service-account-key.pem
    ca.pem	    kubernetes.pem	service-account.pem

<br/>

    $ KUBERNETES_PUBLIC_ADDRESS=10.81.125.83

<br/>

**The kubelet Kubernetes Configuration File**

```
for instance in worker-0 worker-1 worker-2; do
  kubectl config set-cluster kubernetes-the-hard-way \
    --certificate-authority=ca.pem \
    --embed-certs=true \
    --server=https://${KUBERNETES_PUBLIC_ADDRESS}:6443 \
    --kubeconfig=${instance}.kubeconfig

  kubectl config set-credentials system:node:${instance} \
    --client-certificate=${instance}.pem \
    --client-key=${instance}-key.pem \
    --embed-certs=true \
    --kubeconfig=${instance}.kubeconfig

  kubectl config set-context default \
    --cluster=kubernetes-the-hard-way \
    --user=system:node:${instance} \
    --kubeconfig=${instance}.kubeconfig

  kubectl config use-context default --kubeconfig=${instance}.kubeconfig
done
```

<br/>

**The kube-proxy Kubernetes Configuration File**

```
{
  kubectl config set-cluster kubernetes-the-hard-way \
    --certificate-authority=ca.pem \
    --embed-certs=true \
    --server=https://${KUBERNETES_PUBLIC_ADDRESS}:6443 \
    --kubeconfig=kube-proxy.kubeconfig

  kubectl config set-credentials system:kube-proxy \
    --client-certificate=kube-proxy.pem \
    --client-key=kube-proxy-key.pem \
    --embed-certs=true \
    --kubeconfig=kube-proxy.kubeconfig

  kubectl config set-context default \
    --cluster=kubernetes-the-hard-way \
    --user=system:kube-proxy \
    --kubeconfig=kube-proxy.kubeconfig

  kubectl config use-context default --kubeconfig=kube-proxy.kubeconfig
}
```

<br/>

**The kube-controller-manager Kubernetes Configuration File**

```
{
  kubectl config set-cluster kubernetes-the-hard-way \
    --certificate-authority=ca.pem \
    --embed-certs=true \
    --server=https://127.0.0.1:6443 \
    --kubeconfig=kube-controller-manager.kubeconfig

  kubectl config set-credentials system:kube-controller-manager \
    --client-certificate=kube-controller-manager.pem \
    --client-key=kube-controller-manager-key.pem \
    --embed-certs=true \
    --kubeconfig=kube-controller-manager.kubeconfig

  kubectl config set-context default \
    --cluster=kubernetes-the-hard-way \
    --user=system:kube-controller-manager \
    --kubeconfig=kube-controller-manager.kubeconfig

  kubectl config use-context default --kubeconfig=kube-controller-manager.kubeconfig
}
```

<br/>

**The kube-scheduler Kubernetes Configuration File**

```
{
  kubectl config set-cluster kubernetes-the-hard-way \
    --certificate-authority=ca.pem \
    --embed-certs=true \
    --server=https://127.0.0.1:6443 \
    --kubeconfig=kube-scheduler.kubeconfig

  kubectl config set-credentials system:kube-scheduler \
    --client-certificate=kube-scheduler.pem \
    --client-key=kube-scheduler-key.pem \
    --embed-certs=true \
    --kubeconfig=kube-scheduler.kubeconfig

  kubectl config set-context default \
    --cluster=kubernetes-the-hard-way \
    --user=system:kube-scheduler \
    --kubeconfig=kube-scheduler.kubeconfig

  kubectl config use-context default --kubeconfig=kube-scheduler.kubeconfig
}
```

<br/>

**The admin Kubernetes Configuration File**

```
{
  kubectl config set-cluster kubernetes-the-hard-way \
    --certificate-authority=ca.pem \
    --embed-certs=true \
    --server=https://127.0.0.1:6443 \
    --kubeconfig=admin.kubeconfig

  kubectl config set-credentials admin \
    --client-certificate=admin.pem \
    --client-key=admin-key.pem \
    --embed-certs=true \
    --kubeconfig=admin.kubeconfig

  kubectl config set-context default \
    --cluster=kubernetes-the-hard-way \
    --user=admin \
    --kubeconfig=admin.kubeconfig

  kubectl config use-context default --kubeconfig=admin.kubeconfig
}
```

<br/>

**Distribute the Kubernetes Configuration Files**

    $ for instance in worker-0 worker-1 worker-2; do
      lxc file push ${instance}.kubeconfig kube-proxy.kubeconfig ${instance}/root/
    done

<br/>

    $ for instance in controller-0 controller-1 controller-2; do
       lxc file push admin.kubeconfig kube-controller-manager.kubeconfig kube-scheduler.kubeconfig ${instance}/root/
    done

<br/>

**The Encryption Key**

    $ ENCRYPTION_KEY=$(head -c 32 /dev/urandom | base64)

<br/>

**The Encryption Config File**

```
cat > encryption-config.yaml <<EOF
kind: EncryptionConfig
apiVersion: v1
resources:
  - resources:
      - secrets
    providers:
      - aescbc:
          keys:
            - name: key1
              secret: ${ENCRYPTION_KEY}
      - identity: {}
EOF
```

<br/>

    $ for instance in controller-0 controller-1 controller-2; do
       lxc file push encryption-config.yaml ${instance}/root/
    done

<br/>

### controller-0, controller-1, controller-2

    $ lxc exec controller-0 bash

    # wget -q --show-progress --https-only --timestamping \
      "https://github.com/coreos/etcd/releases/download/v3.3.9/etcd-v3.3.9-linux-amd64.tar.gz"

<br/>

```
{
  tar -xvf etcd-v3.3.9-linux-amd64.tar.gz
  sudo mv etcd-v3.3.9-linux-amd64/etcd* /usr/local/bin/
}

```

<br/>

```
{
  sudo mkdir -p /etc/etcd /var/lib/etcd
  sudo cp ca.pem kubernetes-key.pem kubernetes.pem /etc/etcd/
}

```

    // IP меняется для каждого контроллера
    $ INTERNAL_IP=10.81.125.195
    $ ETCD_NAME=$(hostname -s)

Вносим изменения в файл.

```
cat <<EOF | sudo tee /etc/systemd/system/etcd.service
[Unit]
Description=etcd
Documentation=https://github.com/coreos

[Service]
ExecStart=/usr/local/bin/etcd \\
  --name ${ETCD_NAME} \\
  --cert-file=/etc/etcd/kubernetes.pem \\
  --key-file=/etc/etcd/kubernetes-key.pem \\
  --peer-cert-file=/etc/etcd/kubernetes.pem \\
  --peer-key-file=/etc/etcd/kubernetes-key.pem \\
  --trusted-ca-file=/etc/etcd/ca.pem \\
  --peer-trusted-ca-file=/etc/etcd/ca.pem \\
  --peer-client-cert-auth \\
  --client-cert-auth \\
  --initial-advertise-peer-urls https://${INTERNAL_IP}:2380 \\
  --listen-peer-urls https://${INTERNAL_IP}:2380 \\
  --listen-client-urls https://${INTERNAL_IP}:2379,https://127.0.0.1:2379 \\
  --advertise-client-urls https://${INTERNAL_IP}:2379 \\
  --initial-cluster-token etcd-cluster-0 \\
  --initial-cluster controller-0=https://10.81.125.195:2380,controller-1=https://10.81.125.253:2380,controller-2=https://10.81.125.234:2380 \\
  --initial-cluster-state new \\
  --data-dir=/var/lib/etcd
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
EOF
```

<br/>

{
sudo systemctl daemon-reload
sudo systemctl enable etcd
sudo systemctl start etcd
}

<br/>

После выполнения шагов на всех контроллерах, на любом выполнить проверку:

    # ETCDCTL_API=3 etcdctl member list \
      --endpoints=https://127.0.0.1:2379 \
      --cacert=/etc/etcd/ca.pem \
      --cert=/etc/etcd/kubernetes.pem \
      --key=/etc/etcd/kubernetes-key.pem

    5fdfcae5f3a54aec, started, controller-0, https://10.81.125.195:2380, https://10.81.125.195:2379
    6689f80a87ca9772, started, controller-2, https://10.81.125.234:2380, https://10.81.125.234:2379
    e94668eeef47a72c, started, controller-1, https://10.81.125.253:2380, https://10.81.125.253:2379

<br/>

### 06 Bootstrap master nodes

**controller-0, controller-1, controller-2**

    $ lxc exec controller-0 bash

    # mkdir -p /etc/kubernetes/config

<br/>

    # wget -q --show-progress --https-only --timestamping \
      "https://storage.googleapis.com/kubernetes-release/release/v1.12.0/bin/linux/amd64/kube-apiserver" \
      "https://storage.googleapis.com/kubernetes-release/release/v1.12.0/bin/linux/amd64/kube-controller-manager" \
      "https://storage.googleapis.com/kubernetes-release/release/v1.12.0/bin/linux/amd64/kube-scheduler" \
      "https://storage.googleapis.com/kubernetes-release/release/v1.12.0/bin/linux/amd64/kubectl"

<br/>

    # {
      chmod +x kube-apiserver kube-controller-manager kube-scheduler kubectl
      mv kube-apiserver kube-controller-manager kube-scheduler kubectl /usr/local/bin/
    }

**Configure the Kubernetes API Server**

    # {
      mkdir -p /var/lib/kubernetes/

      mv ca.pem ca-key.pem kubernetes-key.pem kubernetes.pem \
        service-account-key.pem service-account.pem \
        encryption-config.yaml /var/lib/kubernetes/
    }

<br/>

    // IP меняетяс для каждого контроллера
    # INTERNAL_IP=10.81.125.195

<br/>

Меняем содержимое конфига

```
cat <<EOF | sudo tee /etc/systemd/system/kube-apiserver.service
[Unit]
Description=Kubernetes API Server
Documentation=https://github.com/kubernetes/kubernetes

[Service]
ExecStart=/usr/local/bin/kube-apiserver \\
  --advertise-address=${INTERNAL_IP} \\
  --allow-privileged=true \\
  --apiserver-count=3 \\
  --audit-log-maxage=30 \\
  --audit-log-maxbackup=3 \\
  --audit-log-maxsize=100 \\
  --audit-log-path=/var/log/audit.log \\
  --authorization-mode=Node,RBAC \\
  --bind-address=0.0.0.0 \\
  --client-ca-file=/var/lib/kubernetes/ca.pem \\
  --enable-admission-plugins=Initializers,NamespaceLifecycle,NodeRestriction,LimitRanger,ServiceAccount,DefaultStorageClass,ResourceQuota \\
  --enable-swagger-ui=true \\
  --etcd-cafile=/var/lib/kubernetes/ca.pem \\
  --etcd-certfile=/var/lib/kubernetes/kubernetes.pem \\
  --etcd-keyfile=/var/lib/kubernetes/kubernetes-key.pem \\
  --etcd-servers=https://10.81.125.195:2379,https:/10.81.125.253:2379,https://10.81.125.234:2379 \\
  --event-ttl=1h \\
  --experimental-encryption-provider-config=/var/lib/kubernetes/encryption-config.yaml \\
  --kubelet-certificate-authority=/var/lib/kubernetes/ca.pem \\
  --kubelet-client-certificate=/var/lib/kubernetes/kubernetes.pem \\
  --kubelet-client-key=/var/lib/kubernetes/kubernetes-key.pem \\
  --kubelet-https=true \\
  --runtime-config=api/all \\
  --service-account-key-file=/var/lib/kubernetes/service-account.pem \\
  --service-cluster-ip-range=10.32.0.0/24 \\
  --service-node-port-range=30000-32767 \\
  --tls-cert-file=/var/lib/kubernetes/kubernetes.pem \\
  --tls-private-key-file=/var/lib/kubernetes/kubernetes-key.pem \\
  --v=2
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
EOF

```

<br/>

    # mv kube-controller-manager.kubeconfig /var/lib/kubernetes/

<br/>

```
cat <<EOF | sudo tee /etc/systemd/system/kube-controller-manager.service
[Unit]
Description=Kubernetes Controller Manager
Documentation=https://github.com/kubernetes/kubernetes

[Service]
ExecStart=/usr/local/bin/kube-controller-manager \\
  --address=0.0.0.0 \\
  --cluster-cidr=10.200.0.0/16 \\
  --cluster-name=kubernetes \\
  --cluster-signing-cert-file=/var/lib/kubernetes/ca.pem \\
  --cluster-signing-key-file=/var/lib/kubernetes/ca-key.pem \\
  --kubeconfig=/var/lib/kubernetes/kube-controller-manager.kubeconfig \\
  --leader-elect=true \\
  --root-ca-file=/var/lib/kubernetes/ca.pem \\
  --service-account-private-key-file=/var/lib/kubernetes/service-account-key.pem \\
  --service-cluster-ip-range=10.32.0.0/24 \\
  --use-service-account-credentials=true \\
  --v=2
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
EOF
```

<br/>

    # mv kube-scheduler.kubeconfig /var/lib/kubernetes/

<br/>

```
cat <<EOF | sudo tee /etc/kubernetes/config/kube-scheduler.yaml
apiVersion: componentconfig/v1alpha1
kind: KubeSchedulerConfiguration
clientConnection:
  kubeconfig: "/var/lib/kubernetes/kube-scheduler.kubeconfig"
leaderElection:
  leaderElect: true
EOF
```

<br/>

```
cat <<EOF | sudo tee /etc/systemd/system/kube-scheduler.service
[Unit]
Description=Kubernetes Scheduler
Documentation=https://github.com/kubernetes/kubernetes

[Service]
ExecStart=/usr/local/bin/kube-scheduler \\
  --config=/etc/kubernetes/config/kube-scheduler.yaml \\
  --v=2
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
EOF

```

<br/>

    # {
      systemctl daemon-reload
      systemctl enable kube-apiserver kube-controller-manager kube-scheduler
      systemctl start kube-apiserver kube-controller-manager kube-scheduler
    }

На каком-нибудь контроллере сделать проверку

Одна нода не поднялась.

    # kubectl get componentstatuses --kubeconfig admin.kubeconfig

    NAME                 STATUS      MESSAGE                                                                                                      ERROR
    etcd-1               Unhealthy   Get https://:2379/health: tls: either ServerName or InsecureSkipVerify must be specified in the tls.Config
    etcd-2               Healthy     {"health":"true"}
    controller-manager   Healthy     ok
    scheduler            Healthy     ok
    etcd-0               Healthy     {"health":"true"}

<br/>

### Выполнить на одной ноде

**RBAC**

```
cat <<EOF | kubectl apply --kubeconfig admin.kubeconfig -f -
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRole
metadata:
  annotations:
    rbac.authorization.kubernetes.io/autoupdate: "true"
  labels:
    kubernetes.io/bootstrapping: rbac-defaults
  name: system:kube-apiserver-to-kubelet
rules:
  - apiGroups:
      - ""
    resources:
      - nodes/proxy
      - nodes/stats
      - nodes/log
      - nodes/spec
      - nodes/metrics
    verbs:
      - "*"
EOF

```

<br/>

```
cat <<EOF | kubectl apply --kubeconfig admin.kubeconfig -f -
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: system:kube-apiserver
  namespace: ""
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: system:kube-apiserver-to-kubelet
subjects:
  - apiGroup: rbac.authorization.k8s.io
    kind: User
    name: kubernetes
EOF
```

<br/>

### 07 Bootstrap worker nodes

На всех worker узлах

    $ lxc exec worker-0 bash

<br/>

    # {
      apt-get update
      apt-get -y install socat conntrack ipset
    }

<br/>

    # wget -q --show-progress --https-only --timestamping \
      https://github.com/kubernetes-sigs/cri-tools/releases/download/v1.12.0/crictl-v1.12.0-linux-amd64.tar.gz \
      https://storage.googleapis.com/kubernetes-the-hard-way/runsc-50c283b9f56bb7200938d9e207355f05f79f0d17 \
      https://github.com/opencontainers/runc/releases/download/v1.0.0-rc5/runc.amd64 \
      https://github.com/containernetworking/plugins/releases/download/v0.6.0/cni-plugins-amd64-v0.6.0.tgz \
      https://github.com/containerd/containerd/releases/download/v1.2.0-rc.0/containerd-1.2.0-rc.0.linux-amd64.tar.gz \
      https://storage.googleapis.com/kubernetes-release/release/v1.12.0/bin/linux/amd64/kubectl \
      https://storage.googleapis.com/kubernetes-release/release/v1.12.0/bin/linux/amd64/kube-proxy \
      https://storage.googleapis.com/kubernetes-release/release/v1.12.0/bin/linux/amd64/kubelet

<br/>

    # mkdir -p \
      /etc/cni/net.d \
      /opt/cni/bin \
      /var/lib/kubelet \
      /var/lib/kube-proxy \
      /var/lib/kubernetes \
      /var/run/kubernetes

<br/>

    # {
        mv runsc-50c283b9f56bb7200938d9e207355f05f79f0d17 runsc
        mv runc.amd64 runc
        chmod +x kubectl kube-proxy kubelet runc runsc
        mv kubectl kube-proxy kubelet runc runsc /usr/local/bin/
        tar -xvf crictl-v1.12.0-linux-amd64.tar.gz -C /usr/local/bin/
        tar -xvf cni-plugins-amd64-v0.6.0.tgz -C /opt/cni/bin/
        tar -xvf containerd-1.2.0-rc.0.linux-amd64.tar.gz -C /
      }

<br/>

    // для worker-0
    # POD_CIDR=10.200.0.0/24

    // для worker-1
    # POD_CIDR=10.200.1.0/24

    // для worker-2
    # POD_CIDR=10.200.2.0/24

<br/>

```
cat <<EOF | sudo tee /etc/cni/net.d/10-bridge.conf
{
    "cniVersion": "0.3.1",
    "name": "bridge",
    "type": "bridge",
    "bridge": "cnio0",
    "isGateway": true,
    "ipMasq": true,
    "ipam": {
        "type": "host-local",
        "ranges": [
          [{"subnet": "${POD_CIDR}"}]
        ],
        "routes": [{"dst": "0.0.0.0/0"}]
    }
}
EOF

```

<br/>

```
cat <<EOF | sudo tee /etc/cni/net.d/99-loopback.conf
{
    "cniVersion": "0.3.1",
    "type": "loopback"
}
EOF

```

<br/>

    # mkdir -p /etc/containerd/

<br/>

```
cat << EOF | sudo tee /etc/containerd/config.toml
[plugins]
  [plugins.cri.containerd]
    snapshotter = "overlayfs"
    [plugins.cri.containerd.default_runtime]
      runtime_type = "io.containerd.runtime.v1.linux"
      runtime_engine = "/usr/local/bin/runc"
      runtime_root = ""
    [plugins.cri.containerd.untrusted_workload_runtime]
      runtime_type = "io.containerd.runtime.v1.linux"
      runtime_engine = "/usr/local/bin/runsc"
      runtime_root = "/run/containerd/runsc"
    [plugins.cri.containerd.gvisor]
      runtime_type = "io.containerd.runtime.v1.linux"
      runtime_engine = "/usr/local/bin/runsc"
      runtime_root = "/run/containerd/runsc"
EOF
```

<br/>

Внесли имзенение в оригинальный файл.
т.к. у индуса что-то не заработало ранее.

```
cat <<EOF | sudo tee /etc/systemd/system/containerd.service
[Unit]
Description=containerd container runtime
Documentation=https://containerd.io
After=network.target

[Service]
ExecStart=/bin/containerd
Restart=always
RestartSec=5
Delegate=yes
KillMode=process
OOMScoreAdjust=-999
LimitNOFILE=1048576
LimitNPROC=infinity
LimitCORE=infinity

[Install]
WantedBy=multi-user.target
EOF
```

<br/>

    # {
      mv ${HOSTNAME}-key.pem ${HOSTNAME}.pem /var/lib/kubelet/
      mv ${HOSTNAME}.kubeconfig /var/lib/kubelet/kubeconfig
      mv ca.pem /var/lib/kubernetes/
    }

<br/>

```
cat <<EOF | sudo tee /var/lib/kubelet/kubelet-config.yaml
kind: KubeletConfiguration
apiVersion: kubelet.config.k8s.io/v1beta1
authentication:
  anonymous:
    enabled: false
  webhook:
    enabled: true
  x509:
    clientCAFile: "/var/lib/kubernetes/ca.pem"
authorization:
  mode: Webhook
clusterDomain: "cluster.local"
clusterDNS:
  - "10.32.0.10"
podCIDR: "${POD_CIDR}"
resolvConf: "/run/systemd/resolve/resolv.conf"
runtimeRequestTimeout: "15m"
tlsCertFile: "/var/lib/kubelet/${HOSTNAME}.pem"
tlsPrivateKeyFile: "/var/lib/kubelet/${HOSTNAME}-key.pem"
EOF
```

<br/>

Внесли имзенение в оригинальный файл.

```
cat <<EOF | sudo tee /etc/systemd/system/kubelet.service
[Unit]
Description=Kubernetes Kubelet
Documentation=https://github.com/kubernetes/kubernetes
After=containerd.service
Requires=containerd.service

[Service]
ExecStart=/usr/local/bin/kubelet \\
  --config=/var/lib/kubelet/kubelet-config.yaml \\
  --container-runtime=remote \\
  --container-runtime-endpoint=unix:///var/run/containerd/containerd.sock \\
  --image-pull-progress-deadline=2m \\
  --kubeconfig=/var/lib/kubelet/kubeconfig \\
  --network-plugin=cni \\
  --register-node=true \\
  --fail-swap-on=false \\
  --v=2
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
EOF
```

<br/>

**Configure the Kubernetes Proxy**

    # mv kube-proxy.kubeconfig /var/lib/kube-proxy/kubeconfig

<br/>

```
cat <<EOF | sudo tee /var/lib/kube-proxy/kube-proxy-config.yaml
kind: KubeProxyConfiguration
apiVersion: kubeproxy.config.k8s.io/v1alpha1
clientConnection:
  kubeconfig: "/var/lib/kube-proxy/kubeconfig"
mode: "iptables"
clusterCIDR: "10.200.0.0/16"
EOF
```

<br/>

```
cat <<EOF | sudo tee /etc/systemd/system/kube-proxy.service
[Unit]
Description=Kubernetes Kube Proxy
Documentation=https://github.com/kubernetes/kubernetes

[Service]
ExecStart=/usr/local/bin/kube-proxy \\
  --config=/var/lib/kube-proxy/kube-proxy-config.yaml
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
EOF
```

<br/>

    {
      systemctl daemon-reload
      systemctl enable containerd kubelet kube-proxy
      systemctl start containerd kubelet kube-proxy
    }

<br/>

    # exit

<br/>

### На клиенте

    $ mkdir ~/.kube
    $ lxc file pull controller-0/root/admin.kubeconfig ~/.kube/config
    $ vi ~/.kube/config

Вместо

    server: https://127.0.0.1:6443

Прописать HaProxy

<br/>

    $ kubectl version --short
    Client Version: v1.14.1
    Server Version: v1.12.0

<br/>

    $ kubectl cluster-info

<br/>

    $ kubectl get nodes
    NAME       STATUS   ROLES    AGE    VERSION
    worker-0   Ready    <none>   29m    v1.12.0
    worker-1   Ready    <none>   4m6s   v1.12.0
    worker-2   Ready    <none>   13s    v1.12.0

<br/>

    $ kubectl get cs
    NAME                 STATUS      MESSAGE                                                                                                      ERROR
    etcd-1               Unhealthy   Get https://:2379/health: tls: either ServerName or InsecureSkipVerify must be specified in the tls.Config
    scheduler            Healthy     ok
    controller-manager   Healthy     ok
    etcd-0               Healthy     {"health":"true"}
    etcd-2               Healthy     {"health":"true"}

<br/>

### POD Network Routes

Сессия 1:

    $ watch kubectl get pods -o wide

Сессия 2:

    $ kubectl run myshell1 -it --rm --image busybox -- sh
    $ kubectl run myshell2 -it --rm --image busybox -- sh


    # hostname -i

    В общем в контейнерах пинг не проходит между контейнерами.

<br/>

Чего-то я этот шаг совсем не понял. Мы же это на клиенте выполняли?

<br/>
    // gw -- worker nodes
    $ sudo route add -net 10.200.0.0 netmask 255.255.255.0 gw 10.81.125.9
    $ sudo route add -net 10.200.1.0 netmask 255.255.255.0 gw 10.81.125.231
    $ sudo route add -net 10.200.2.0 netmask 255.255.255.0 gw 10.81.125.10

    $ ip route show

<br/>

**Deploying the DNS Cluster Add-on**

    $ kubectl -n kube-system get all
    No resources found.

<br/>

    $ kubectl apply -f https://storage.googleapis.com/kubernetes-the-hard-way/coredns.yaml

<br/>

    $ kubectl -n kube-system get all
    NAME                           READY   STATUS    RESTARTS   AGE
    pod/coredns-699f8ddd77-rwlbm   1/1     Running   0          30s
    pod/coredns-699f8ddd77-sxv9v   1/1     Running   0          30s

    NAME               TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)         AGE
    service/kube-dns   ClusterIP   10.32.0.10   <none>        53/UDP,53/TCP   30s

    NAME                      DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
    deployment.apps/coredns   2         2         2            2           30s

    NAME                                 DESIRED   CURRENT   READY   AGE
    replicaset.apps/coredns-699f8ddd77   2         2         2       30s

<br/>

**Smoke Test**

    $ kubectl create secret generic kubernetes-the-hard-way \
      --from-literal="mykey=mydata"

    $ lxc exec controller-0 bash

<br/>

    # ETCDCTL_API=3 etcdctl get \
      --endpoints=https://127.0.0.1:2379 \
      --cacert=/etc/etcd/ca.pem \
      --cert=/etc/etcd/kubernetes.pem \
      --key=/etc/etcd/kubernetes-key.pem\
      /registry/secrets/default/kubernetes-the-hard-way | hexdump -C

Увидели справа k8s:enc. Оч. обрадовались.

    # exit

<br/>

    $ kubectl run nginx --image=nginx

    $ kubectl get pods
    NAME                    READY   STATUS    RESTARTS   AGE
    nginx-dbddb74b8-9mw8k   1/1     Running   0          86s


    $ kubectl port-forward nginx-dbddb74b8-9mw8k 8080:80

http://localhost:8080/

OK

<br/>

    $ kubectl scale deploy nginx --replicas 10

<br/>

    $ kubectl get pods -o wide
    NAME                    READY   STATUS    RESTARTS   AGE     IP           NODE       NOMINATED NODE
    nginx-dbddb74b8-94wng   1/1     Running   0          34s     10.200.1.3   worker-1   <none>
    nginx-dbddb74b8-9mw8k   1/1     Running   0          4m14s   10.200.2.5   worker-2   <none>
    nginx-dbddb74b8-9vmwk   1/1     Running   0          33s     10.200.2.8   worker-2   <none>
    nginx-dbddb74b8-d7hnd   1/1     Running   0          33s     10.200.2.7   worker-2   <none>
    nginx-dbddb74b8-jfrt7   1/1     Running   0          34s     10.81.0.6    worker-0   <none>
    nginx-dbddb74b8-k9j26   1/1     Running   0          34s     10.200.1.4   worker-1   <none>
    nginx-dbddb74b8-mgm7r   1/1     Running   0          33s     10.81.0.8    worker-0   <none>
    nginx-dbddb74b8-mz9qh   1/1     Running   0          33s     10.200.1.5   worker-1   <none>
    nginx-dbddb74b8-wplbh   1/1     Running   0          33s     10.81.0.7    worker-0   <none>
    nginx-dbddb74b8-wq4q8   1/1     Running   0          33s     10.200.2.6   worker-2   <none>

<br/>

    $ kubectl expose deployment nginx --port 80 --type NodePort
    $ kubectl get svc

http://worker-ip:exposed_port

<br/>

### Удаление всего

    $ kubectl delete svc nginx
    $ kubectl delete deploy nginx

<br/>

    $ lxc stop --all

    $ {
      lxc delete haproxy --force
      lxc delete controller-0 --force
      lxc delete controller-1 --force
      lxc delete controller-2 --force
      lxc delete worker-0 --force
      lxc delete worker-1 --force
      lxc delete worker-2 --force
    }

    $ lxc list

<br/>

### Kubernetes The Hard Way HA Testing

https://www.youtube.com/watch?v=AWGSchaLnJA
