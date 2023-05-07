---
layout: page
title: Замена виртуального диска VirtualBox в командной строке
description: Замена виртуального диска VirtualBox в командной строке
keywords: Замена виртуального диска VirtualBox в командной строке
permalink: /adm/virtual/virtualbox/replace-disk/
---

# Замена виртуального диска VirtualBox в командной строке

Нужно заменить диск виртуальной машины hfpd3.vmdk
Тупо заменить диск не получилось.

    $ VBoxHeadless --startvm ${vm}

    ***

    Error: failed to start machine. Error message: UUID {00000000-0000-0000-0000-000000000000} of the medium '/mnt/dsk1/machines/hadoop/hadoop/hfpd3.vmdk' does not match the value {979fe763-da73-4cad-97ef-b7fb29ac48e2} stored in the media registry ('/home/vmadm/.config/VirtualBox/VirtualBox.xml')

<br/>

    $ VBoxManage list hdds

    ***

    UUID:           979fe763-da73-4cad-97ef-b7fb29ac48e2
    Parent UUID:    base
    State:          inaccessible
    Type:           normal (base)
    Location:       /mnt/dsk1/machines/hadoop/hadoop/hfpd3.vmdk
    Storage format: VMDK
    Capacity:       0 MBytes

<br/>

    $ VBoxManage storageattach ${vm} \
    --storagectl "SAS Controller" \
    --port 0 \
    --type hdd \
    --medium none

<br/>

    $ VBoxManage closemedium disk 979fe763-da73-4cad-97ef-b7fb29ac48e2

<br/>

    $ VBoxManage storageattach ${vm} \
    --storagectl "SAS Controller" \
    --port 0 \
    --type hdd \
    --medium hfpd3.vmdk

<br/>

    $ VBoxHeadless --startvm ${vm}
