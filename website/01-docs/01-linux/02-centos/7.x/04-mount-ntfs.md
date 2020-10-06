---
layout: page
title: Смонтировать NTFS в Centos 7.x
description: Смонтировать NTFS в Centos 7.x
keywords: Смонтировать NTFS в Centos 7.x
permalink: /linux/centos/7.x/mount-ntfs/
---

# Смонтировать NTFS в Centos 7.x

<br/>

    # wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm

    # rpm -ivh epel-release-latest-7.noarch.rpm

    # yum clean all && yum update

    # yum install -y ntfs-3g ntfsprogs

n
<br/>

### При необходимости смотрировать в режими только для чтения:

<br/>

-- Если нужно найти диск

    # fdisk -l

<br/>

-- Создаем каталог и монтируем

    # mkdir -p /mnt/ntfs
    # mount -o ro /dev/sda4 /mnt/ntfs

sda4 - мой диск. У тебя может быть и другой!

<br/>

Подробнее:

https://www.techbrown.com/mount-ntfs-file-system-centos-7-rhel-7.shtml
