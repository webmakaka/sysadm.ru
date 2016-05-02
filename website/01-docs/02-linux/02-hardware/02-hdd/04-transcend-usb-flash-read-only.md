---
layout: page
title: Transcend 8GB USB MP3 Player работает в режиме только для чтения
permalink: /linux/hardware/hdd/transcend-usb-flash-read-only/
---

Девайс так себе.
Если вытаскивать из USB - иногда виснет.
Но бывает что и похуже. Например флешка начинает работать в режиме только для чтения.

Чтобы вернуть работоспособность, ее можно отформатировать.
Я попробовал в Windows, не получилось.

В Linux уже несколько раз делал. Чтобы в следующий раз когда такое произойдет не вспоминать команды, пожалуй запишу их здесь.


Главное не перепутать диск и не отформатировать коллекцию фильмов.

    # fdisk -l /dev/sde
    Note: sector size is 1024 (not 512)

    Disk /dev/sde: 8344 MB, 8344002560 bytes

<br/>

    # dd if=/dev/zero of=/dev/sde

И забивается нулями флешка, часов этак 8, наверное.


    # fdisk /dev/sde
    Note: sector size is 1024 (not 512)
    Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabel
    Building a new DOS disklabel with disk identifier 0xa548d5cd.
    Changes will remain in memory only, until you decide to write them.
    After that, of course, the previous content won't be recoverable.

    Warning: invalid flag 0x0000 of partition table 4 will be corrected by w(rite)

    Command (m for help): n
    Partition type:
       p   primary (0 primary, 0 extended, 4 free)
       e   extended
    Select (default p): p
    Partition number (1-4, default 1):
    Using default value 1
    First sector (1024-8148439, default 1024):
    Using default value 1024
    Last sector, +sectors or +size{K,M,G} (1024-8148439, default 8148439):
    Using default value 8148439

    Command (m for help): w
    The partition table has been altered!

    Calling ioctl() to re-read partition table.
    Syncing disks.

<br/>

    # fdisk /dev/sde

    Command (m for help): t
    Selected partition 1
    Hex code (type L to list codes): b
    Changed system type of partition 1 to b (W95 FAT32)

    Command (m for help): w
    The partition table has been altered!

    Calling ioctl() to re-read partition table.

    WARNING: If you have created or modified any DOS 6.x
    partitions, please see the fdisk manual page for additional
    information.
    Syncing disks.

<br/>

    # mkfs.vfat /dev/sde1
    mkfs.fat 3.0.26 (2014-03-07)


Все.  
Можно пользоваться.
