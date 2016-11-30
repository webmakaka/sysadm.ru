---
layout: page
title: VirtialBox Подключение USB устройств
permalink: /linux/virtual/virtualbox/usb/
---


# VirtialBox Подключение USB устройств


Работал с USB устройствами на виртуальной машине не очень много. Похоже, каждый раз для подключения usb устройства приходится удалять предыдущее значение и указывать новое (если меняется устройство).


// Проверяю, установлен ли Extension Pack

    $ VBoxManage list extpacks

    Extension Packs: 1

    Pack no. 0:   Oracle VM VirtualBox Extension Pack

    Version:          4.2.6

    Revision:         82870

    Edition:          

    Description:  USB 2.0 Host Controller, VirtualBox RDP, PXE ROM with E1000 support.

    VRDE Module:  VBoxVRDP

    Usable:           true

    Why unusable:


// Включаю

    $ VBoxManage modifyvm $vm --usb on --usbehci on

    $ VBoxManage list usbhost

<br/>

Если в списке отсутствуют usb устройства, а они реально имеются

    $ lsusb

    Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
    Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
    Bus 003 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
    Bus 004 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
    Bus 005 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
    Bus 006 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
    Bus 007 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
    Bus 008 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
    Bus 009 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
    Bus 010 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
    Bus 008 Device 002: ID 046d:c05f Logitech, Inc.
    Bus 008 Device 003: ID 046e:5503 Behavior Tech. Computer Corp.
    Bus 009 Device 002: ID 18d1:4ee1 Google Inc.
    Bus 009 Device 003: ID 0458:010e KYE Systems Corp. (Mouse Systems)


<br/>

Нужно добавить пользователя в группу vboxusers

    # usermod -a -G vboxusers vmadm

<br/>

    $ VBoxManage list usbhost

    Находим интересующее нас устройство

    UUID:                   621b053c-dc5d-425b-bae8-e6c80616d7f9

    VendorId:               0x18d1 (18D1)

    ProductId:              0x4ee1 (4EE1)

    Revision:               2.38 (0238)

    Port:                   0

    USB version/speed:  2/2

    Manufacturer:           samsung

    Product:                Nexus 10

    SerialNumber:           R32D103PK8K

    Address:                sysfs:/sys/devices/pci0000:00/0000:00:02.0/0000:02:00.0/usb9/9-1//device:/dev/vboxusb/009/004

    Current State:          Busy


<br/>

Подключаем:

    $ VBoxManage \
    usbfilter add 0 \
    --target $vm \
    --name usbstick \
    --vendorid 18D1 \
    --productid 4EE1

<br/>

// Стартуем виртуальную машину. (Данные будут доступны уже после старта)

    $ VBoxHeadless --startvm ${vm}

    $ VBoxManage showvminfo $vm

<br/>

Смотрим:

    Currently Attached USB Devices:

    UUID:                   9075d004-d291-4318-922a-1db7b6cc6a00

    VendorId:               0x18d1 (18D1)

    ProductId:              0x4ee1 (4EE1)

    Revision:               2.38 (0238)

    Manufacturer:           samsung

    Product:                Nexus 10

    SerialNumber:           R32D103PK8K

    Address:                sysfs:/sys/devices/pci0000:00/0000:00:02.0/0000:02:00.0/usb9/9-1//device:/dev/vboxusb/009/004

<br/>

// Удалить usb устройство

    $ VBoxManage usbfilter remove 0  --target $vm


</pre>
