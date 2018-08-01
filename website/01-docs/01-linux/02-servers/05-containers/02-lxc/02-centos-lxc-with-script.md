---
layout: page
title: Linux Containers (lxc) (Настройка с помощью скрипта)
permalink: /linux/servers/containers/lxc/centos-lxc-with-script/
---


# Linux Containers (lxc) (Настройка с помощью скрипта)


<pre>


<strong>
При выключенном selinux возникают ошибки. Вроде он не работает с выключенным selinux.</strong>

<strong>Включаю selinux</strong>


[root@server1 ~]# sestatus
SELinux status:                 disabled


[root@server1 ~]# vi /etc/sysconfig/selinux

SELINUX=disabled
меняю на
SELINUX=enforcing


<strong>Установка и настройка Linux Containers (lxc)</strong>

[root@server1 ~]# yum install -y libvirt libvirt-client python-virtinst


[root@server1 ~]# chkconfig --level 345 libvirtd on
[root@server1 ~]# service libvirtd restart

[root@server1 ~]# chkconfig --level 345 cgconfig on
[root@server1 ~]# service cgconfig restart


[root@server1 ~]# cat /proc/mounts | grep cgroup
cgroup /cgroup/cpuset cgroup rw,relatime,cpuset 0 0
cgroup /cgroup/cpu cgroup rw,relatime,cpu 0 0
cgroup /cgroup/cpuacct cgroup rw,relatime,cpuacct 0 0
cgroup /cgroup/memory cgroup rw,relatime,memory 0 0
cgroup /cgroup/devices cgroup rw,relatime,devices 0 0
cgroup /cgroup/freezer cgroup rw,relatime,freezer 0 0
cgroup /cgroup/net_cls cgroup rw,relatime,net_cls 0 0
cgroup /cgroup/blkio cgroup rw,relatime,blkio 0 0


-- Если не перезагрузиться, возникает ошибка
[root@server1 ~]# reboot

======================================


[root@server1 ~]# yum install -y rsync wget


[root@server1 ~]# cd /tmp/
[root@server1 tmp]# wget https://gist.github.com/sysadm-ru/8833424/raw/69bb0fcededb9a98aaebcd35e6efc7e90c1ca85e/create_libvirt_lxc_guest.sh

</pre>

Содержание скрипта:
<br/><br/>

<script src="https://gist.github.com/sysadm-ru/8833424.js"></script>

<pre>
[root@server1 tmp]# chmod +x create_libvirt_lxc_guest.sh


[root@server1 tmp]# ./create_libvirt_lxc_guest.sh test


[root@server1 tmp]# cd /containers/test
# virsh -c lxc:/// create libvirt.xml


[root@server1 test]# virsh -c lxc:/// list --all
 ID    Имя                         Статус
----------------------------------------------------
 1463  test                           работает



</pre>
