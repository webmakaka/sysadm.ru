---
layout: page
title: Commands
permalink: /linux/servers/containers/kubernetes/commands/
---

# Commands

    # kubectl get nodes -o wide

    NAME STATUS ROLES AGE VERSION INTERNAL-IP EXTERNAL-IP OS-IMAGE KERNEL-VERSION CONTAINER-RUNTIME
    master.k8s NotReady master 94s v1.14.0 192.168.0.10 <none> CentOS Linux 7 (Core) 3.10.0-957.5.1.el7.x86_64 docker://1.13.1
    node1.k8s NotReady <none> 69s v1.14.0 192.168.0.11 <none> CentOS Linux 7 (Core) 3.10.0-957.5.1.el7.x86_64 docker://1.13.1
    node2.k8s NotReady <none> 71s v1.14.0 192.168.0.12 <none> CentOS Linux 7 (Core) 3.10.0-957.5.1.el7.x86_64 docker://1.13.1
