---
layout: page
title: Kubernetes > Второе знакомство
permalink: /linux/containers/kubernetes/second-look/
---


# Kubernetes > Второе знакомство


Видео устарело!

<div align="center">

    <iframe width="640" height="360" src="https://www.youtube.com/embed/DC7NECq3Ghs" frameborder="0" allowfullscreen></iframe>

</div>

<br/>

https://github.com/kubernetes/kubernetes/blob/master/docs/devel/local-cluster/docker.md


<br/>

    1) Run etcd
    2) Run the master
    3) Run the service proxy

<br/>

    $  lsb_release -a
    No LSB modules are available.
    Distributor ID:	Ubuntu
    Description:	Ubuntu 14.04.5 LTS
    Release:	14.04
    Codename:	trusty


<br/>

    1) Устанавливаю docker >= 1.11


<br/>

    # mkdir -p /var/lib/kubelet
    # mount --bind /var/lib/kubelet /var/lib/kubelet
    # mount --make-shared /var/lib/kubelet

<br/>

    // current stable version of Kubernetes

    # export K8S_VERSION=$(curl -sS https://storage.googleapis.com/kubernetes-release/release/stable.txt)

<br/>

    # echo $K8S_VERSION
    v1.5.1

<br/>


    export ARCH=amd64

<br/>

    docker run -d \
        --volume=/sys:/sys:rw \
        --volume=/var/lib/docker/:/var/lib/docker:rw \
        --volume=/var/lib/kubelet/:/var/lib/kubelet:rw,shared \
        --volume=/var/run:/var/run:rw \
        --net=host \
        --pid=host \
        --privileged \
        --name=kubelet \
        gcr.io/google_containers/hyperkube-${ARCH}:${K8S_VERSION} \
        /hyperkube kubelet \
            --hostname-override=127.0.0.1 \
            --api-servers=http://localhost:8080 \
            --config=/etc/kubernetes/manifests \
            --cluster-dns=10.0.0.10 \
            --cluster-domain=cluster.local \
            --allow-privileged --v=2


<br/>


    # docker ps
    CONTAINER ID        IMAGE                                             COMMAND                  CREATED             STATUS              PORTS               NAMES
    9f70eda0ee0c        gcr.io/google_containers/hyperkube-amd64:v1.5.1   "/setup-files.sh IP:1"   4 seconds ago       Up 4 seconds                            k8s_setup.23c83c39_k8s-master-127.0.0.1_kube-system_4394c19dda877c24b884cbf295aa0f8b_05192fd7
    5f1d7ac2fca6        gcr.io/google_containers/hyperkube-amd64:v1.5.1   "/hyperkube scheduler"   5 seconds ago       Up 4 seconds                            k8s_scheduler.c23703ed_k8s-master-127.0.0.1_kube-system_4394c19dda877c24b884cbf295aa0f8b_eeeddd71
    35c94bfd7495        gcr.io/google_containers/hyperkube-amd64:v1.5.1   "/hyperkube controlle"   5 seconds ago       Up 4 seconds                            k8s_controller-manager.b57d587e_k8s-master-127.0.0.1_kube-system_4394c19dda877c24b884cbf295aa0f8b_1214c0f5
    3d8275564dfb        gcr.io/google_containers/pause-amd64:3.0          "/pause"                 5 seconds ago       Up 4 seconds                            k8s_POD.d8dbe16c_k8s-master-127.0.0.1_kube-system_4394c19dda877c24b884cbf295aa0f8b_c1334180
    47537ee0e02c        gcr.io/google_containers/pause-amd64:3.0          "/pause"                 6 seconds ago       Up 5 seconds                            k8s_POD.d8dbe16c_k8s-etcd-127.0.0.1_kube-system_f89dc4ffa30d2912ab90b232e1abbd6f_b2347b4b
    ba490e66f8a3        gcr.io/google_containers/hyperkube-amd64:v1.5.1   "/copy-addons.sh sing"   7 seconds ago       Up 6 seconds                            k8s_kube-addon-manager-data.2f5f205f_kube-addon-manager-127.0.0.1_kube-system_4e74caf80cabad0e1402ad38779cea66_d325f7cb
    f1b24f69ad53        gcr.io/google_containers/pause-amd64:3.0          "/pause"                 7 seconds ago       Up 6 seconds                            k8s_POD.d8dbe16c_kube-addon-manager-127.0.0.1_kube-system_4e74caf80cabad0e1402ad38779cea66_66ad09e7
    af65cbdf9f35        gcr.io/google_containers/hyperkube-amd64:v1.5.1   "/hyperkube proxy --m"   8 seconds ago       Up 7 seconds                            k8s_kube-proxy.a1074855_k8s-proxy-127.0.0.1_kube-system_897b77833b5bf921ea0895756c1d0b6c_d04dd906
    2aa9824e14b0        gcr.io/google_containers/pause-amd64:3.0          "/pause"                 8 seconds ago       Up 7 seconds                            k8s_POD.d8dbe16c_k8s-proxy-127.0.0.1_kube-system_897b77833b5bf921ea0895756c1d0b6c_eadc4fec
    fd76a4abeff5        gcr.io/google_containers/hyperkube-amd64:v1.5.1   "/hyperkube kubelet -"   15 seconds ago      Up 14 seconds                           kubelet



<br/>

### Download kubectl


    # curl -sSL "https://storage.googleapis.com/kubernetes-release/release/${K8S_VERSION}/bin/linux/amd64/kubectl" > /usr/bin/kubectl

<br/>

    # chmod +x /usr/bin/kubectl

<br/>


    # kubectl config set-cluster test-doc --server=http://localhost:8080
    # kubectl config set-context test-doc --cluster=test-doc
    # kubectl config use-context test-doc


<br/>

### Test it out

    # kubectl get nodes
    NAME        STATUS    AGE
    127.0.0.1   Ready     6m


<br/>

### Run an application

    # kubectl run nginx --image=nginx --port=80

<br/>

### Expose it as a service

    # kubectl expose deployment nginx --port=80

<br/>

    # ip=$(kubectl get svc nginx --template={{.spec.clusterIP}})

<br/>

    # echo $ip
    10.0.0.86

<br/>

    # curl $ip
    <!DOCTYPE html>
    <html>
    <head>
    <title>Welcome to nginx!</title>
    <style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
    </style>
    </head>
    <body>
    <h1>Welcome to nginx!</h1>
    <p>If you see this page, the nginx web server is successfully installed and
    working. Further configuration is required.</p>

    <p>For online documentation and support please refer to
    <a href="http://nginx.org/">nginx.org</a>.<br/>
    Commercial support is available at
    <a href="http://nginx.com/">nginx.com</a>.</p>

    <p><em>Thank you for using nginx.</em></p>
    </body>
    </html>
