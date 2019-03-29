---
layout: page
title: Варианты инсталляции kubernetes
permalink: /linux/servers/containers/kubernetes/install/
---

# Варианты инсталляции kubernetes

Kubernetes can be installed using different configurations. The four major installation types are briefly presented below:

-   All-in-One Single-Node Installation
    With all-in-one, all the master and worker components are installed on a single node. This is very useful for learning, development, and testing. This type should not be used in production. Minikube is one such example.

-   Single-Node etcd, Single-Master, and Multi-Worker Installation
    In this setup, we have a single master node, which also runs a single-node etcd instance. Multiple worker nodes are connected to the master node.

-   Single-Node etcd, Multi-Master, and Multi-Worker Installation
    In this setup, we have multiple master nodes, which work in an HA mode, but we have a single-node etcd instance. Multiple worker nodes are connected to the master nodes.

-   Multi-Node etcd, Multi-Master, and Multi-Worker Installation
    In this mode, etcd is configured in a clustered mode, outside the Kubernetes cluster, and the nodes connect to it. The master nodes are all configured in an HA mode, connecting to multiple worker nodes. This is the most advanced and recommended production setup.
