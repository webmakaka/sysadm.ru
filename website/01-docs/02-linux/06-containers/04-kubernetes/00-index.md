---
layout: page
title: Kubernetes (НИЧЕГО НЕ РАБОТАЕТ!)
permalink: /linux/containers/kubernetes/
---


# Kubernetes (НИЧЕГО НЕ РАБОТАЕТ!)



https://www.youtube.com/watch?v=9uDivcfCdUA


http://containertutorials.com/get_started_kubernetes/index.html


    $ sudo su -

    # apt-get update
    # apt-get install -y ssh curl traceroute docker.io python python3





    # ssh-keygen -t rsa

     [Enter]

     # cat /root/.ssh/id_rsa.pub >> /root/.ssh/authorized_keys



     // Может так???
     $ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys


     $ ssh root@127.0.0.1


     # cd /opt/



    # wget https://github.com/GoogleCloudPlatform/kubernetes/releases/download/v1.0.1/kubernetes.tar.gz


    <!-- https://github.com/kubernetes/kubernetes/archive/release-1.5.zip -->

    # tar -xvf kubernetes.tar.gz
    # cd kubernetes/cluster/ubuntu

    # ./build.sh

    # ls binaries


    # cd ../ubuntu
    # cp config-default.sh config-default.sh.orig

    # vi config-default.sh

    export nodes="root@127.0.0.1"
    export roles="ai"
    export NUM_MINIONS=${NUM_MINIONS:-1}

    # cd ../


//********************

    # etcd --version
    etcd Version: 2.0.12
    Git SHA: 5686c33
    Go Version: go1.4.2
    Go OS/Arch: linux/amd64

    # systemctl start etcd
    # systemctl status etcd

//********************


    # cd /opt/kubernetes/cluster/
    # KUBERNETES_PROVIDER=ubuntu ./kube-up.sh


    error: couldn't read version from server: Get http://127.0.0.1:8080/api: dial tcp 127.0.0.1:8080: connection refused

    Waiting for 1 ready nodes. 0 ready nodes, 0 registered. Retrying.
    error: couldn't read version from server: Get http://127.0.0.1:8080/api: dial tcp 127.0.0.1:8080: connection refused

    Waiting for 1 ready nodes. 0 ready nodes, 0 registered. Retrying.
    error: couldn't read version from server: Get http://127.0.0.1:8080/api: dial tcp 127.0.0.1:8080: connection refused

    Waiting for 1 ready nodes. 0 ready nodes, 0 registered. Retrying.
    error: couldn't read version from server: Get http://127.0.0.1:8080/api: dial tcp 127.0.0.1:8080: connection refused

    Waiting for 1 ready nodes. 0 ready nodes, 0 registered. Retrying.



// -------------------------------




    $ export PATH=$PATH:/opt/kubernetes/cluster/ubuntu/binaries

    $ kubectl get nodes
    error: couldn't read version from server: Get http://localhost:8080/api: dial tcp 127.0.0.1:8080: connection refused




        # export PATH=$PATH:/opt/bin


<br/>

### Что дальше не заработало:




<br/>

### И это тоже не работает!:


<div align="center">

    <iframe width="560" height="315" src="https://www.youtube.com/embed/DC7NECq3Ghs" frameborder="0" allowfullscreen></iframe>

</div>

1) Run etcd
2) Run the master
3) Run the service proxy



    # curl -sSL "https://storage.googleapis.com/kubernetes-release/release/{{page.version}}.0/bin/linux/amd64/kubectl" > /usr/bin/kubectl

    # chmod +x /usr/bin/kubectl


    $ kubectl config set-cluster test-doc --server=http://localhost:8080
    $ kubectl config set-context test-doc --cluster=test-doc
    $ kubectl config use-context test-doc


https://github.com/kubernetes/kubernetes/blob/master/docs/devel/local-cluster/docker.md


    # mkdir -p /var/lib/kubelet
    # mount --bind /var/lib/kubelet /var/lib/kubelet
    # mount --make-shared /var/lib/kubelet


    $ export K8S_VERSION=$(curl -sS https://storage.googleapis.com/kubernetes-release/release/stable.txt)

    $ export ARCH=amd64

    $ docker run -d \
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

    $ docker ps
    CONTAINER ID        IMAGE                                             COMMAND                  CREATED             STATUS              PORTS               NAMES
    d5bc2279962f        gcr.io/google_containers/pause-amd64:3.0          "/pause"                 3 seconds ago       Up 3 seconds                            k8s_POD.d8dbe16c_kube-addon-manager-127.0.0.1_kube-system_afca367e5b2c2ef0e95bcdbdb10ef64a_6f4f0a51
    f7fb48bd7e70        gcr.io/google_containers/hyperkube-amd64:v1.4.7   "/hyperkube proxy --m"   4 seconds ago       Up 3 seconds                            k8s_kube-proxy.b0fa485a_k8s-proxy-127.0.0.1_kube-system_3b0f48c440b584b0b476b68b4db3e463_d633fdef
    ff817cae1561        gcr.io/google_containers/pause-amd64:3.0          "/pause"                 4 seconds ago       Up 3 seconds                            k8s_POD.d8dbe16c_k8s-proxy-127.0.0.1_kube-system_3b0f48c440b584b0b476b68b4db3e463_35106400
    0da909ee4147        gcr.io/google_containers/hyperkube-amd64:v1.4.7   "/hyperkube kubelet -"   12 seconds ago      Up 12 seconds                           kubelet


<br/>


    $ kubectl get nodes

    ничего

    $ kubectl run nginx --image=nginx --port=80

    ничего


=======================

http://kubernetes.io/docs/getting-started-guides/minikube/
