---
layout: page
title: Создание виртуальной машины virtula box для курса по hadoop
permalink: /linux/distributed-systems/hadoop/crate-virtual-mashine-virtual-box-for-hadoop-course/
---

Настраивается, так как я всегда и настраиваю virtulabox машины. Кому нужно, смотрите подробности в разделе по virtualbox.


    $ vm=hadoop

<br/>

    $ mkdir -p ${VM_HOME}/${vm}/snapshots

<br/>

    $ VBoxManage createvm \
    --name ${vm} \
    --ostype Ubuntu_64 \
    --basefolder ${VM_HOME}/${vm} \
    --register

<br/>

    $ VBoxManage modifyvm ${vm} --memory 4096

<br/>

    $ VBoxManage modifyvm ${vm} --vram 32

<br/>

    $ VBoxManage modifyvm ${vm} --floppy disabled --audio none

<br/>

    $ VBoxManage storagectl ${vm} \
    --add sas \
    --name "SAS Controller"

<br/>

    $ mv hfpd3.vmdk /mnt/dsk1/machines/hadoop/hadoop/

<br/>

    $ VBoxManage storageattach ${vm} \
    --storagectl "SAS Controller" \
    --port 0 \
    --type hdd \
    --medium hfpd3.vmdk

<br/>

    $ VBoxManage modifyvm ${vm} \
    --nictype1 82540EM \
    --nic1 bridged \
    --bridgeadapter1 eth1

<br/>

    $ VBoxManage modifyvm ${vm} \
    --nictype2 82540EM \
    --nic2 bridged \
    --bridgeadapter2 eth1

<br/>

    $ VBoxManage modifyvm ${vm} \
    --boot1 disk \
    --boot2 dvd

<br/>

    $ VBoxManage modifyvm ${vm} \
    --snapshotfolder ${VM_HOME}/${vm}/snapshots

<br/>

    $ VBoxManage modifyvm ${vm} \
    --vrde on \
    --vrdemulticon on \
    --vrdeauthtype null \
    --vrdeaddress 192.168.1.5 \
    --vrdeport 3389

<br/>

    $ VBoxHeadless --startvm ${vm}

<br/>

    $ remmina



Далее установил Guest Additions.


Рекомендую сделать точку отката. Сам не сделал, пришлось заново все ставить.
