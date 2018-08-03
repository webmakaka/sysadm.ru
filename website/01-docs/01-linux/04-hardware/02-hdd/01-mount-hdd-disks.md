---
layout: page
title: Монтирование hdd
permalink: /linux/hardware/hdd/mount-disks/
---


# Монтирование hdd


    # fdisk -l /dev/sda


    -- Если нужно создать раздел. (см. что спрашивает.)
    # fdisk /dev/sda


    Command (m for help): [n]
    Partition type:
       p   primary (0 primary, 0 extended, 4 free)
       e   extended
    Select (default p): [p]
    Partition number (1-4, default 1):
    Using default value 1
    First sector (1024-8148439, default 1024):
    Using default value 1024
    Last sector, +sectors or +size{K,M,G} (1024-8148439, default 8148439):
    Using default value 8148439

    Command (m for help): [w]
    The partition table has been altered!

    Calling ioctl() to re-read partition table.
    Syncing disks.


    -- Запись на созданный раздел фаловой системы
    # mkfs.ext4 /dev/sda1

<br/>


    # ls /dev/sd*
    /dev/sda   /dev/sdb   /dev/sdb2  /dev/sdb4
    /dev/sda1  /dev/sdb1  /dev/sdb3  /dev/sdb5


<br/>


    # mkdir /mnt/dsk1


 <br/>  


     # blkid /dev/sda1
     /dev/sda1: UUID="ff8ed3b5-a750-4a49-b02a-8f56ea418797" TYPE="ext4"


или

     # blkid
     /dev/sda1: UUID="ff8ed3b5-a750-4a49-b02a-8f56ea418797" TYPE="ext4"
     /dev/sdb1: UUID="1f5a5c6d-25b6-4496-997b-2abdb95983e2" TYPE="xfs"
     /dev/sdb2: UUID="0c52e673-2e67-4565-b507-62a8940e1eb2" TYPE="swap"
     /dev/sdb3: UUID="4437af40-4b7a-4fa1-b6a3-48559f6dfcb2" TYPE="xfs"
     /dev/sdb5: UUID="fc1de487-ffaf-414c-82d1-765597229a8a" TYPE="xfs"


<br/>

    # mount /dev/sda1 /mnt/dsk1/

<br/>

    # mount /mnt/dsk1/


<br/>

### Запись в fstab (чтобы после каждой загрузки не монтировать заново)


    # vi /etc/fstab

<br/>

    # 1 TB
    UUID=ff8ed3b5-a750-4a49-b02a-8f56ea418797 /mnt/dsk1 ext4 defaults 0 0


<br/>

    # mount /mnt/dsk1/


<br/>

Отменяем резервирование 5% для суперпользователя следующей командой. (Если не нужно)

    # tune2fs /dev/sda1 -m 0


<br/>

    # df -h
    ***
    /dev/sda1       917G   77M  917G   1% /mnt/dsk1
