---
layout: page
title: Создание виртуальной машины VirtualBox с Oracle Linux 6.X. в командной строке linux для инсталляции базы данных Oracle
description: Создание виртуальной машины VirtualBox с Oracle Linux 6.X. в командной строке linux для инсталляции базы данных Oracle
keywords: Создание виртуальной машины VirtualBox с Oracle Linux 6.X. в командной строке linux для инсталляции базы данных Oracle
permalink: /linux/virtual/virtualbox/vm/oracle-linux-6/
---

# Создание виртуальной машины VirtualBox с Oracle Linux 6.X. в командной строке linux для инсталляции базы данных Oracle

Имеем компьютер, подключенный к свичу по ethernet.
На него нужно установить виртуальную машину с Oracle Linux, приблизительно похожий на сервер для этой базы.

---

Тоже самое, что и с виртуальной машиной с Centos. Только дисков поболее, памяти, сетевые интерфейсы другие.

---

    # su - vmadm

<br/>

    $ vm=vm_oel57_oradb112

<br/>

    $ VBoxManage createvm \
    --name ${vm} \
    --ostype Oracle_64 \
    --basefolder ${VM_HOME}/${vm} \
    --register

### Устанавливаю материнскую пату:

Выбираю материнскую пату с более современным чипсетом. По умолчанию piix3

    $ VBoxManage modifyvm ${vm}  --chipset ich9

### Устанавливаю процессор:

    $ VBoxManage modifyvm ${vm} --cpus 2

### Устанавливаем планку оперативной памяти:

    $ VBoxManage modifyvm ${vm} --memory 4608

### Создание и подключение жестких дисков:

Создаю виртуальные жесткие диски. Размер (size), рекомендуется задавать согласно имеющихся ресурсов. Иначе возможны проблемы и крах виртуальной машины):

    $ cd ${VM_HOME}/${vm}/${vm}

<br/>

    $ VBoxManage createhd \
    --filename ${vm}_dsk1.vdi \
    --size 40960 \
    --format VDI \
    --variant Standard

    $ VBoxManage createhd \
    --filename ${vm}_dsk2.vdi \
    --size 40960 \
    --format VDI \
    --variant Standard

    $ VBoxManage createhd \
    --filename ${vm}_dsk3.vdi \
    --size 40960 \
    --format VDI \
    --variant Standard

    $ VBoxManage createhd \
    --filename ${vm}_dsk4.vdi \
    --size 40960 \
    --format VDI \
    --variant Standard

    $ VBoxManage createhd \
    --filename ${vm}_dsk5.vdi \
    --size 40960 \
    --format VDI \
    --variant Standard

    $ VBoxManage createhd \
    --filename ${vm}_dsk6.vdi \
    --size 40960 \
    --format VDI \
    --variant Standard

    $ VBoxManage createhd \
    --filename ${vm}_dsk7.vdi \
    --size 40960 \
    --format VDI \
    --variant Standard

    $ VBoxManage createhd \
    --filename ${vm}_dsk8.vdi \
    --size 40960 \
    --format VDI \
    --variant Standard

### Подключаю диски к SAS контроллеру (максимум 8):

    $ VBoxManage storageattach ${vm} \
    --storagectl "SAS Controller" \
    --port 0 \
    --type hdd \
    --medium ${vm}_dsk1.vdi

    $ VBoxManage storageattach ${vm} \
    --storagectl "SAS Controller" \
    --port 1 \
    --type hdd \
    --medium ${vm}_dsk2.vdi

    $ VBoxManage storageattach ${vm} \
    --storagectl "SAS Controller" \
    --port 2 \
    --type hdd \
    --medium ${vm}_dsk3.vdi

    $ VBoxManage storageattach ${vm} \
    --storagectl "SAS Controller" \
    --port 3 \
    --type hdd \
    --medium ${vm}_dsk4.vdi

    $ VBoxManage storageattach ${vm} \
    --storagectl "SAS Controller" \
    --port 4 \
    --type hdd \
    --medium ${vm}_dsk5.vdi

    $ VBoxManage storageattach ${vm} \
    --storagectl "SAS Controller" \
    --port 5 \
    --type hdd \
    --medium ${vm}_dsk6.vdi

    $ VBoxManage storageattach ${vm} \
    --storagectl "SAS Controller" \
    --port 6 \
    --type hdd \
    --medium ${vm}_dsk7.vdi

    $ VBoxManage storageattach ${vm} \
    --storagectl "SAS Controller" \
    --port 7 \
    --type hdd \
    --medium ${vm}_dsk8.vdi

### Подключение сетевых интерфейсов:

Наберите команду;

    $ VBoxManage list bridgedifs

Обратите внимание на значение:

Name: eth0

Я использую eth0 как основной физический интерфейс, который будут использовать виртуальные машины в качестве моста.

Подключаю к виртуальной машине 3 виртуальных сетевых “Intel® 82540EM Gigabit Ethernet Controller”, работающих как bridget (3 адаптера нужные в случае необходимости установить RAC):

    $ VBoxManage modifyvm ${vm} \
    --nictype1 82540EM \
    --nic1 bridged \
    --bridgeadapter1 eth0

    $ VBoxManage modifyvm ${vm} \
    --nictype2 82540EM \
    --nic2 bridged \
    --bridgeadapter2 eth0

    $ VBoxManage modifyvm ${vm} \
    --nictype3 82540EM \
    --nic3 bridged \
    --bridgeadapter3 eth0
