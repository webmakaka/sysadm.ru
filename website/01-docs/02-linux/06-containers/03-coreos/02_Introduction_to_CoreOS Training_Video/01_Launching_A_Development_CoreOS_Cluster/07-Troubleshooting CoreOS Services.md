---
layout: page
title: Troubleshooting CoreOS Services
permalink: /linux/containers/coreos/Introduction_to_CoreOS/Launching_A_Development_CoreOS_Cluster/Troubleshooting_CoreOS_Services/
---


# [O’Reilly Media / Infinite Skills] Introduction to CoreOS Training Video [2015, ENG] : Launching A Development CoreOS Cluster : Troubleshooting CoreOS Services



### Troubleshooting CoreOS Services


    $ systemctl status -l fleet
    ● fleet.service - fleet daemon
       Loaded: loaded (/usr/lib/systemd/system/fleet.service; disabled; vendor prese
      Drop-In: /run/systemd/system/fleet.service.d
               └─20-cloudinit.conf
       Active: active (running) since Sun 2016-11-27 00:57:06 UTC; 2h 20min ago
     Main PID: 831 (fleetd)
        Tasks: 7
       Memory: 24.9M
          CPU: 1min 41.107s
       CGroup: /system.slice/fleet.service
               └─831 /usr/bin/fleetd

    Nov 27 02:40:15 core-02 fleetd[831]: INFO manager.go:259: Removing systemd unit
    Nov 27 02:40:15 core-02 fleetd[831]: INFO manager.go:182: Instructing systemd to
    Nov 27 02:40:15 core-02 fleetd[831]: INFO reconcile.go:330: AgentReconciler comp
    Nov 27 02:40:15 core-02 fleetd[831]: INFO reconcile.go:330: AgentReconciler comp
    Nov 27 02:43:05 core-02 fleetd[831]: INFO manager.go:246: Writing systemd unit n
    Nov 27 02:43:05 core-02 fleetd[831]: INFO manager.go:182: Instructing systemd to
    Nov 27 02:43:05 core-02 fleetd[831]: INFO manager.go:127: Triggered systemd unit
    Nov 27 02:43:05 core-02 fleetd[831]: INFO reconcile.go:330: AgentReconciler comp
    Nov 27 02:43:05 core-02 fleetd[831]: INFO reconcile.go:330: AgentReconciler comp
    Nov 27 02:43:05 core-02 fleetd[831]: INFO reconcile.go:330: AgentReconciler comp


<br/>

    $ systemctl status -l etcd
    ● etcd.service - etcd
       Loaded: loaded (/usr/lib/systemd/system/etcd.service; static; vendor preset:
       Active: inactive (dead)


<br/>

       $ journalctl -b -u etcd
       -- No entries --
