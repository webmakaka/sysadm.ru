---
layout: page
title: Ubuntu - Монтирование жестких дисков
description: Ubuntu - Монтирование жестких дисков
keywords: Ubuntu - Монтирование жестких дисков
permalink: /desktop/linux/ubuntu/setup/mount-hdd/
---

# [Ubuntu] Монтирование жестких дисков



```
// Можно все здесь посмотреть
$ gnome-disks
```

<br/>


```
$ sudo fdisk -l /dev/sd*
```



<br/>


```
$ sudo mkdir -p /mnt/dsk1
$ sudo mkdir -p /mnt/dsk2
```



```
$ blkid /dev/sda1
```

<br/>


```
Samsung SSD 850 PRO 512GB (EXM02B6Q)
9b0094b9-5436-46f4-a9a4-0bf547328f36


WDC WD5000AAKS-00UU3A0 (01.03B01)
8e53f2c6-0c0c-4db5-89ed-4935d89c11de
```

<br/>


```
$ sudo vi /etc/fstab
```

<br/>

```
***
# Samsung SSD 850 PRO 512GB (EXM02B6Q)
UUID=9b0094b9-5436-46f4-a9a4-0bf547328f36 /mnt/dsk1 ext4 defaults 0 0

# WDC WD5000AAKS-00UU3A0 (01.03B01)
UUID=8e53f2c6-0c0c-4db5-89ed-4935d89c11de /mnt/dsk2 ext4 defaults 0 0
```

<br/>


```
$ sudo mount /mnt/dsk1/
$ sudo mount /mnt/dsk2/
```


<br/>

```
$ sudo chown -R ${USER} /mnt/dsk1
$ sudo chown -R ${USER} /mnt/dsk2
```