---
layout: page
title: Инсталляция популярных программ в Centos 7.x
permalink: /linux/basics/centos/7/install-popular-programms/
---


# Инсталляция популярных программ в Centos 7.x

<br/>

### Инсталляция Chrome в Centos 7.x

<br/>

    # vi /etc/yum.repos.d/google-chrome.repo

<br/>

    [google-chrome]
    name=google-chrome
    baseurl=http://dl.google.com/linux/chrome/rpm/stable/$basearch
    enabled=1
    gpgcheck=1
    gpgkey=https://dl-ssl.google.com/linux/linux_signing_key.pub

<br/>

    # yum install google-chrome-stable



<br/>

### Инсталляция VLC Centos 7.x


    # rpm -Uvh https://download1.rpmfusion.org/free/el/rpmfusion-free-release-7.noarch.rpm

<br/>

    # yum install -y vlc


<br/>

### Skype

    # vi /etc/yum.repos.d/skype-stable.repo

<br/>

    [skype-stable]
    name=skype (stable)
    baseurl=https://repo.skype.com/rpm/stable/
    enabled=1
    gpgcheck=1
    gpgkey=https://repo.skype.com/data/SKYPE-GPG-KEY


<br/>

    # yum install -y skype




<br/>

### 7zip

Подключить репо <a href="/linux/basics/centos/7/repos/">Epel</a>

<br/>

    # yum install -y p7zip
