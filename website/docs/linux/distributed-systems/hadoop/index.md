---
layout: page
title: Hadoop
permalink: /linux/distributed-systems/hadoop/
---

По курсу:

### [O'Reilly Media] Hadoop Fundamentals for Data Scientists Training Video [2015, ENG]

Можно к разбору материалов из видео.

Почему никто с нуля Hadoop не ставит?
Уже несколько видео встречаю, где работают уже с готовыми виртуалками.

Виртуальная машина:

https://www.dropbox.com/s/eg80qsitun7txu1/hfpd3.vmdk.gz?dl=0




> Альтернативно, можно поискать Cloudera CDH5 или Hortonworks Sandbox


~/.bash_aliases алиасы на некоторые команды.



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

<br/>

    user: student  
    password: password

<br/>

Я залогинился как Hadoop Analyst

Ставлю Guest Additions
