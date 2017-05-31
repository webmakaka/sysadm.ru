---
layout: page
title: Работа с репозиториями в Centos 7
permalink: /linux/basics/centos/7/working-with-repos/
---

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

    # ls etc/yum.repos.d/

Просто удалить ненужный файл.

<br/>

-- обновить

    # yum clean all && yum update
