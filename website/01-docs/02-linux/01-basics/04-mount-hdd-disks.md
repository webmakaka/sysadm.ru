---
layout: page
title: Монтирование hdd
permalink: /linux/basics/mount-hdd-disks/
---


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

### Запись в fstab


    # vi /etc/fstab

<br/>

    # 1 TB
    UUID=ff8ed3b5-a750-4a49-b02a-8f56ea418797 /mnt/dsk1 ext4 defaults 0 0


<br/>

    # mount /mnt/dsk1/


<br/>

Отменяем резервирование 5% для суперпользователя следующей командой.

    # tune2fs /dev/sda1 -m 0


<br/>

    # df -h
    ***
    /dev/sda1       917G   77M  917G   1% /mnt/dsk1
