---
layout: page
title: Kubernetes
permalink: /linux/servers/containers/kubernetes/
---

# Kubernetes

Kubernetes converts a set of computers into one big one.


**Бесплатный курс по kubernetes:**  
https://courses.edx.org/courses/course-v1:LinuxFoundationX+LFS158x+1T2018/course/

<br/>

**Building blocks:**

- RCs - Replication Controllers  
- PODS - минимальная единица управляемая RC. A Pod is a logical collection of one or more containers
- Services

<br/>

**Kubernets:**

- One instance for the Master
- Serveral instances as Minions


<br/>

**Текущая стабильная версия:**

$ echo $(curl -sS https://storage.googleapis.com/kubernetes-release/release/stable.txt)
v1.10.0

<br/>

### Tutorials  

http://kubernetes.io/docs/tutorials/

<br/>

### Creating a Custom Cluster from Scratch  

https://kubernetes.io/docs/getting-started-guides/scratch/


<br/>

### Kubernetes The Hard Way

https://github.com/kelseyhightower/kubernetes-the-hard-way

<br/>

### Варианты инсталляции kubernetes

Kubernetes can be installed using different configurations. The four major installation types are briefly presented below:

* All-in-One Single-Node Installation
With all-in-one, all the master and worker components are installed on a single node. This is very useful for learning, development, and testing. This type should not be used in production. Minikube is one such example.

* Single-Node etcd, Single-Master, and Multi-Worker Installation
In this setup, we have a single master node, which also runs a single-node etcd instance. Multiple worker nodes are connected to the master node.

* Single-Node etcd, Multi-Master, and Multi-Worker Installation
In this setup, we have multiple master nodes, which work in an HA mode, but we have a single-node etcd instance. Multiple worker nodes are connected to the master nodes.

* Multi-Node etcd, Multi-Master, and Multi-Worker Installation
In this mode, etcd is configured in a clustered mode, outside the Kubernetes cluster, and the nodes connect to it. The master nodes are all configured in an HA mode, connecting to multiple worker nodes. This is the most advanced and recommended production setup.




<br/>

### Первые попытки установить и запустить локально


Особого смысла локально разворачивать нет. Обычно достаточно установить minikube и радоваться. А потом когда придет понимание, возможно поработать с облачными решениями. Возможно, что собственный локальный кластер и не понадобится никогда.


[Какие-то попытки разобраться с cubectl и minikube](/linux/servers/containers/kubernetes/cubect-minikube/) 


[Kubernetes - A Multi-Tier Application](/linux/servers/containers/kubernetes/multi-tier-application/) 


[Kubernetes > Первое знакомство (устарело)](/linux/servers/containers/kubernetes/first-look/)  

[Kubernetes > Устанавливаем локальный кластер (устарело)](/linux/servers/containers/kubernetes/local-cluster/)  


<br/>

### На rutracker мне написали:

8-ми узловой кластер прекрасно работает на локальной машине с 16GB RAM!

На Ubuntu ставится одной командой:
conjure-up kubernetes

https://kubernetes.io/docs/getting-started-guides/ubuntu/local/

Если интересует зачем - смотрим сюда:  
Fission: Serverless Functions as a Service for Kubernetes  
https://github.com/fission/fission
