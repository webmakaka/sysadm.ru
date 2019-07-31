---
layout: page
title: Архиваторы в Linux
permalink: /linux/desktops/archives/installation/
---

# Инсталляция архиваторов в Linux

## Ubuntu

-- 7 zip

    # apt-get install -y p7zip-full

## Centos

-- По идее, ставится так:

    # wget http://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-10.noarch.rpm
    # yum install -y rar unrar

-- rar из исходников (не проверял !!!)

    wget http://www.rarlab.com/rar/rarlinux-3.8.0.tar.gz
    tar -zxvf rarlinux-3.8.0.tar.gz
    cd rar
    su root
    make
    make install
    exit

<br/>

### 7zip

Подключить репо <a href="/linux/desktops/centos/7.x/repos/">Epel</a>

<br/>

    # yum install -y p7zip
