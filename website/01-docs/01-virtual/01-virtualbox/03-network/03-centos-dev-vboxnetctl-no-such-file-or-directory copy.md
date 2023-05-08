---
layout: page
title: Centos 7 VBoxManage hostonlyif create - /dev/vboxnetctl No such file or directory
description: Centos 7 VBoxManage hostonlyif create - /dev/vboxnetctl No such file or directory
keywords: Centos 7 VBoxManage hostonlyif create - /dev/vboxnetctl No such file or directory
permalink: /virtual/virtualbox/network/centos-dev-vboxnetctl-no-such-file-or-directory/
---

# Oracle linux 7 (тоже что и Centos 7): VBoxManage hostonlyif create - /dev/vboxnetctl No such file or directory

Делаю:  
15.02.2018

    # VBoxManage -v
    5.2.6r120293

<br/>

    # VBoxManage hostonlyif create
    0%...
    Progress state: NS_ERROR_FAILURE
    VBoxManage: error: Failed to create the host-only adapter
    VBoxManage: error: VBoxNetAdpCtl: Error while adding new interface: failed to open /dev/vboxnetctl: No such file or directory
    VBoxManage: error: Details: code NS_ERROR_FAILURE (0x80004005), component HostNetworkInterfaceWrap, interface IHostNetworkInterface
    VBoxManage: error: Context: "RTEXITCODE handleCreate(HandlerArg*)" at line 94 of file VBoxManageHostonly.cpp

<br/>

    $ ls /dev/vboxnetctl
    ls: cannot access /dev/vboxnetctl: No such file or directory

<br/>

    # /sbin/rcvboxdrv setup
    vboxdrv.sh: Stopping VirtualBox services.
    vboxdrv.sh: Building VirtualBox kernel modules.
    This system is currently not set up to build kernel modules.
    Please install the Linux kernel "header" files matching the current kernel
    for adding new hardware support to the system.
    The distribution packages containing the headers are probably:
        kernel-uek-devel kernel-uek-devel-4.1.12-112.14.13.el7uek.x86_64

<br/>

    # dnf install -y kernel-uek-devel-4.1.12-112.14.13.el7uek.x86_64

<br/>

    # reboot

<br/>

    # /sbin/rcvboxdrv setup
    vboxdrv.sh: Stopping VirtualBox services.
    vboxdrv.sh: Building VirtualBox kernel modules.
    This system is currently not set up to build kernel modules.
    Please install the Linux kernel "header" files matching the current kernel
    for adding new hardware support to the system.
    The distribution packages containing the headers are probably:
        kernel-uek-devel kernel-uek-devel-4.1.12-112.14.15.el7uek.x86_64

<br/>

    # dnf install -y kernel-uek-devel-4.1.12-112.14.15.el7uek.x86_64

<br/>

    # /sbin/rcvboxdrv setupvboxdrv.sh: Stopping VirtualBox services.
    vboxdrv.sh: Building VirtualBox kernel modules.
    vboxdrv.sh: Starting VirtualBox services.

<br/>

    # VBoxManage hostonlyif create
    0%...10%...20%...30%...40%...50%...60%...70%...80%...90%...100%
    Interface 'vboxnet1' was successfully created

<br/>

    # ifconfig vboxnet0
    vboxnet0: flags=4098<BROADCAST,MULTICAST>  mtu 1500
            ether 0a:00:27:00:00:00  txqueuelen 1000  (Ethernet)
            RX packets 0  bytes 0 (0.0 B)
            RX errors 0  dropped 0  overruns 0  frame 0
            TX packets 0  bytes 0 (0.0 B)
            TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
