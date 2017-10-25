---
layout: page
title: Публичные репозитории Centos
permalink: /linux/basics/centos/7/repos/
---

<br/>

# Публичные репозитории Centos 7


<br/>

Стандартный репо:

    # vi /etc/yum.repos.d/centos7.repo

<br/>

    [Centos]
    name= Centos $releasever - $basearch
    baseurl=http://mirror.centos.org/centos/7/os/x86_64/
    gpgkey=http://mirror.centos.org/centos/RPM-GPG-KEY-CentOS-7
    gpgcheck=1
    enabled=1


<br/>


### EPEL Repository

<br/>

    # vi /etc/yum.repos.d/Epel-Centos7.repo

<br/>

    [Epel]
    name= Epel $releasever - $basearch
    baseurl=https://mirror.yandex.ru/epel/7/x86_64/
    gpgkey=https://mirror.yandex.ru/epel/RPM-GPG-KEY-EPEL-7
    gpgcheck=1
    enabled=1


<br/>

Или можно

    # rpm -Uvh http://fedora-mirror01.rbc.ru/pub/epel/epel-release-latest-7.noarch.rpm

<br/>

### rpmfusion

  # yum localinstall -y --nogpgcheck https://download1.rpmfusion.org/free/el/rpmfusion-free-release-7.noarch.rpm https://download1.rpmfusion.org/nonfree/el/rpmfusion-nonfree-release-7.noarch.rpm


Подробнее:

    https://rpmfusion.org/Configuration/


<br/>

**RPMForge/RepoForge both projects are dead and should not be used – Please use EPEL Repository.**



<br/>

# Работа с репозиториями в Centos 7

<br/>

-- получить список

    # yum repolist
    # yum repolist all

<br/>

-- Получить информацию по пакету

    # yum info vlc


<br/>

-- удалить репо

    # ls /etc/yum.repos.d/

Просто удалить ненужный файл.

<br/>

-- обновить

    # yum clean all && yum update
