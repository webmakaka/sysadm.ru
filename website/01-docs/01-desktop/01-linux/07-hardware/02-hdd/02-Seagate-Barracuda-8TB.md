---
layout: page
title: Подключаем диск 8TB в Centos 6.X
description: Подключаем диск 8TB в Centos 6.X
keywords: Подключаем диск 8TB в Centos 6.X
permalink: /desktop/linux/hardware/hdd/seagate/8tb/
---

# Подключаем диск 8TB в Centos 6.X

В общем я вставил диск Seagate Barracuda на 8TB. И по старой доброй привычке создал раздел, отформатировал начал работать.

Скопировал на него гигов 500 информации. Потом еще 1,5 TB данных.
И тут получил сообщение о том что места недостаточно.

Блин, думаю. Ну как так? Диск же на 8TB. Посмотрел, создался раздел размером только на 2TB.

В общем для нормальной работы дисков большего объема, придется использовать parted.

<br/>

```
# fdisk -l /dev/sdc

Disk /dev/sdc: 8001.6 GB, 8001563222016 bytes
255 heads, 63 sectors/track, 972801 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
Disk identifier: 0xbed6eb22

    Device Boot      Start         End      Blocks   Id  System
```

<br/>

```
# yum install -y parted
```

<br/>

```
# parted -a optimal /dev/sdc mklabel gpt mkpart primary 0% 100%
```

<!--
# parted /dev/sdc
GNU Parted 2.1
Using /dev/sdc
Welcome to GNU Parted! Type 'help' to view a list of commands.
(parted)

<br/>

(parted) mklabel gpt
Warning: The existing disk label on /dev/sdc will be destroyed and all data on
this disk will be lost. Do you want to continue?
Yes/No? yes

<br/>

(parted) unit TB

<br/>

(parted) mkpart primary 0 100%

<br/>

(parted) print
Model: ATA ST8000AS0002-1NA (scsi)
Disk /dev/sdc: 8.00TB
Sector size (logical/physical): 512B/4096B
Partition Table: gpt

Number  Start   End     Size    File system  Name     Flags
 1      0.00TB  0.00TB  0.00TB               primary


<br/>

 (parted) quit

-->

<br/>

```
# mkfs.ext4 /dev/sdc1
```

<br>

Получаю UUID диска.

```
# blkid /dev/sdc1
/dev/sdc1: UUID="8e53f2c6-0c0c-4db5-89ed-4935d89c11de" TYPE="ext4"
```

Добавляю запись в fstab, чтобы после перезагрузки раздел диска автоматически смонтировался в файловой системе.

<br/>

```
# vi /etc/fstab
```

Собственно запись

```
# 8 TB HDD
UUID=8e53f2c6-0c0c-4db5-89ed-4935d89c11de /mnt/dsk4 ext4 defaults 0 0
```

<br/>

Далее уже команды как вводил в консоль.

```
# mount /mnt/dsk4
```

<br/>

```
# df -h
***
/dev/sdc1             7.2T   51M  6.8T   1% /mnt/dsk4
***
```

<br/>

Чего-то овердохуя потеряно места!

Причины следующие;

В первую очередь это из-за того, что по умолчанию при создании файловой системы ext4 резервируется 5% раздела, доступные только root. Нужно, чтобы пользователи не забили всё место, если это системный диск.

Отменяем резервирование 5% для суперпользователя следующей командой.

<br/>

```
# tune2fs /dev/sdc1 -m 0
```

После этого:

```
/dev/sdc1             7.2T   51M  7.2T   1% /mnt/dsk4
```
