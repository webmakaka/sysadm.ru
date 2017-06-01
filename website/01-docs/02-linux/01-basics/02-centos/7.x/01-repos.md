---
layout: page
title: Публичные репозитории Centos
permalink: /linux/basics/centos/7/repos/
---

<br/>

# Публичные репозитории Centos

<br/>

**RPMForge/RepoForge both projects are dead and should not be used – Please use EPEL Repository.**

<br/>

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


    # rpm -Uvh http://fedora-mirror01.rbc.ru/pub/epel/epel-release-latest-7.noarch.rpm

<br/>


### rpmfusion

    # rpm -Uvh https://download1.rpmfusion.org/free/el/rpmfusion-free-release-7.noarch.rpm





<br/>


### Что осталось от предыдущей версии статьи:

<pre>


<strong>Yandex</strong>

# vi /etc/yum.repos.d/yandex.repo

[YANDEX]
name= Centos $releasever - $basearch
baseurl=http://mirror.yandex.ru/centos/6.5/os/x86_64/
gpgkey=http://mirror.yandex.ru/centos/6.5/os/x86_64/RPM-GPG-KEY-CentOS-6
gpgcheck=1
enabled=1



<strong>Билайн (Корбина)</strong>
http://mirror.corbina.net/centos/


<strong>RPMForge Repository</strong>

## RHEL/CentOS 6 32 Bit OS ##
# wget http://packages.sw.be/rpmforge-release/rpmforge-release-0.5.2-2.el6.rf.i686.rpm
# rpm -Uvh rpmforge-release-0.5.2-2.el6.rf.i686.rpm

## RHEL/CentOS 6 64 Bit OS ##
# wget http://packages.sw.be/rpmforge-release/rpmforge-release-0.5.2-2.el6.rf.x86_64.rpm
# rpm -Uvh rpmforge-release-0.5.2-2.el6.rf.x86_64.rpm


<strong>EPEL Repository</strong>

## RHEL/CentOS 6 32-Bit ##
# wget http://download.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm
# rpm -ivh epel-release-6-8.noarch.rpm

## RHEL/CentOS 6 64-Bit ##
# wget http://download.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
# rpm -ivh epel-release-6-8.noarch.rpm



# vi /etc/yum.repos.d/repos.repo

[Centos]
name= Centos $releasever - $basearch
baseurl=http://mirror.centos.org/centos/6.5/os/x86_64/
gpgkey=http://mirror.centos.org/centos/6.5/os/x86_64/RPM-GPG-KEY-CentOS-6
gpgcheck=1
enabled=1



[webtatic.com]
name=repo.webtatic.com $releasever - $basearch
baseurl=http://repo.webtatic.com/yum/el6/x86_64
gpgkey=http://repo.webtatic.com/yum/RPM-GPG-KEY-webtatic-andy
gpgcheck=1
enabled=1


[centos.alt.ru]
name=centos.alt.ru $releasever - $basearch
baseurl=http://centos.alt.ru/repository/centos/6/x86_64/
gpgkey=http://centos.alt.ru/repository/centos/RPM-GPG-KEY-CentALT
gpgcheck=1
enabled=1


curl -O http://www6.atomicorp.com/channels/atomic/centos/6/x86_64/RPMS/atomic-release-1.0-14.el6.art.noarch.rpm
rpm -Uvh atomic-release-1.0-14.el6.art.noarch.rpm



</pre>



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

-- добавить (например какой-то репо)

    # rpm -Uvh http://li.nux.ro/download/nux/dextop/el7/x86_64/nux-dextop-release-0-5.el7.nux.noarch.rpm



<br/>

-- удалить

    # ls /etc/yum.repos.d/

Просто удалить ненужный файл.

<br/>

-- обновить

    # yum clean all && yum update
