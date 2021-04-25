---
layout: page
title: Монтирование использовавшегося жесткого диска в linux
description: Монтирование использовавшегося жесткого диска в linux
keywords: Linux, монтирование диска
permalink: /desktop/linux/hardware/hdd/mount-disks-used/
---

# Монтирование использовавшегося жесткого диска в linux

Делаю!  
25.04.2021

В Ubuntu 20.04

```
$ ls /dev/sd*
/dev/sda  /dev/sda1  /dev/sdb  /dev/sdb1  /dev/sdb2
```

<br/>

    # fdisk -l /dev/sda

<br/>

Мне нужно примонтировать диск, подключенный как sda. На нем важные для меня данные. Файловая система Linux

<br/>

    # mkdir /mnt/dsk1

 <br/>

    # blkid /dev/sda1
    /dev/sda1: UUID="8e2cd4e1-6262-4f0a-9d62-204c369d14b1" TYPE="ext4" PARTUUID="bcff8a34-01"

<br/>

    # mount /dev/sda1 /mnt/dsk1/

<br/>

    -- Если нужно отмонтировать
    # mount /mnt/dsk1/

<br/>

### Запись в fstab (чтобы после каждой загрузки не монтировать заново)

    # vi /etc/fstab

<br/>

```shell
# 1 TB
UUID=8e2cd4e1-6262-4f0a-9d62-204c369d14b1 /mnt/dsk1 ext4 defaults 0 0
```

<br/>

    # mount /mnt/dsk1/

<br/>

# Разрешу пользователю писать на диск

    # chown -R <username> /mnt/dsk1

Все
