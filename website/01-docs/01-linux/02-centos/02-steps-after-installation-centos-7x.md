---
layout: page
title: Centos 7 нужные мне для работы пакеты
permalink: /linux/install/centos/6/steps-after-installation-centos-7x/
---

# Centos 7 нужные мне для работы пакеты


// vim

    # yum install -y vim


// ifconfig

    # yum install -y net-tools

// traceroute

    # yum install -y traceroute

// screen

    # yum install -y screen

// lsof

    # yum install -y lsof



<br/>

### После установки, после перезагрузки выводилось приглашение выбрать ядро и все. Не очень удобно, когда работаешь удаленно. Вылечилось следующим образом.


### Grub

    # vi /etc/default/grub

timeout: 0


To make this change active run the command

    # grub2-mkconfig -o /boot/grub2/grub.cfg


<br>

Ну и всякие команды, выбора ядра:

    # awk -F\' '$1=="menuentry " {print $2}' /etc/grub2.cfg
    CentOS Linux (3.10.0-327.10.1.el7.x86_64) 7 (Core)
    CentOS Linux (3.10.0-327.el7.x86_64) 7 (Core)
    CentOS Linux (0-rescue-b612889e6c8048c28b5d8d154acd3b67) 7 (Core)


<br/>

    # grub2-editenv list
    saved_entry=CentOS Linux (3.10.0-327.10.1.el7.x86_64) 7 (Core)

<br/>

    # grub2-set-default 2
