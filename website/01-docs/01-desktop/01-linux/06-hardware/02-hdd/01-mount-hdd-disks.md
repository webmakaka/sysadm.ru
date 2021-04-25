---
layout: page
title: Монтирование жесткого диска в linux
description: Монтирование жесткого диска в linux
keywords: Монтирование жесткого диска в linux
permalink: /desktop/linux/hardware/hdd/mount-disks/
---

# Монтирование жесткого диска в linux

Делаю!  
11.08.2019

В Ubuntu 18.04

!!! На диске нет никаких данных.

    # fdisk -l /dev/sd*

<br/>

    # ls /dev/sd*
    /dev/sda  /dev/sda1  /dev/sda2  /dev/sdb

<br/>

Мне нужно примонтировать диск, подключенный как sdb. На нем нет никаких разделов.

<br/>

    # fdisk /dev/sdb

<br/>

    Command (m for help): [n]
    Partition number (1-128, default 1): [p]
    Value out of range.
    Partition number (1-128, default 1): [1]
    First sector (34-1953525134, default 2048): [Enter]
    Last sector, +sectors or +size{K,M,G,T,P} (2048-1953525134, default 1953525134): [Enter]

    Created a new partition 1 of type 'Linux filesystem' and of size 931,5 GiB.

    Command (m for help): [w]
    The partition table has been altered.
    Calling ioctl() to re-read partition table.
    Syncing disks.

<br/>

    -- Запись на созданный раздел фаловой системы
    # mkfs.ext4 /dev/sdb1

<br/>

    # ls /dev/sd*
    /dev/sda  /dev/sda1  /dev/sda2  /dev/sdb  /dev/sdb1

<br/>

    # mkdir /mnt/dsk1

 <br/>

    # blkid /dev/sdb1
    /dev/sdb1: UUID="66bda136-6e40-478b-87cd-f80e871b5ac3" TYPE="ext4" PARTUUID="8f991ad1-bb8b-f843-853a-946142c288b3"

или

     # blkid
    /dev/sdb1: UUID="66bda136-6e40-478b-87cd-f80e871b5ac3" TYPE="ext4" PARTUUID="8f991ad1-bb8b-f843-853a-946142c288b3"
    ***

<br/>

    # mount /dev/sdb1 /mnt/dsk1/

<br/>

    -- Если нужно отмонтировать
    # umount /mnt/dsk1/

<br/>

### Запись в fstab (чтобы после каждой загрузки не монтировать заново)

    # vi /etc/fstab

<br/>

```shell
# 1 TB
UUID=66bda136-6e40-478b-87cd-f80e871b5ac3 /mnt/dsk1 ext4 defaults 0 0
```

<br/>

    # mount /mnt/dsk1/

<br/>

Отменяем резервирование 5% для суперпользователя следующей командой. (Если не нужно)

    # tune2fs /dev/sdb1 -m 0

<br/>

    # df -h
    ***
    /dev/sdb1       916G   77M  916G   1% /mnt/dsk1

<br/>

# Разрешу пользователю писать на диск

    # chown -R <username> /mnt/dsk1
