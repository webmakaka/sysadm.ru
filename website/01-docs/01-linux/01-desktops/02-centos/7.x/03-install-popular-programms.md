---
layout: page
title: Инсталляция популярных программ в Centos 7.x
permalink: /linux/desktops/centos/7.x/install-popular-programms/
---

# Инсталляция популярных программ в Centos 7.x

<br/>

### Инсталляция Chrome в Centos 7.x

<br/>

    # vi /etc/yum.repos.d/google-chrome.repo

<br/>

```
[google-chrome]
name=google-chrome
baseurl=http://dl.google.com/linux/chrome/rpm/stable/$basearch
enabled=1
gpgcheck=1
gpgkey=https://dl-ssl.google.com/linux/linux_signing_key.pub
```

<br/>

    # yum install -y google-chrome-stable

<br/>

### Инсталляция VLC Centos 7.x

Нужны репо epel, rpmfusion.

[Как добавить репо](/linux/desktops/centos/7.x/repos/)

<br/>

    # yum install -y vlc

<br/>

### Skype

    # vi /etc/yum.repos.d/skype-stable.repo

<br/>

```
[skype-stable]
name=skype (stable)
baseurl=https://repo.skype.com/rpm/stable/
enabled=1
gpgcheck=1
gpgkey=https://repo.skype.com/data/SKYPE-GPG-KEY
```


<br/>

    # dnf install -y skype

<br/>
    -- Удалить skype
    # dnf remove -y skypeforlinux.x86_64

    -- Установить из командной строки, скачанный с сайта skype
    # dnf install -y ./skypeforlinux-64.rpm

<br/>

### Архиваторы

<a href="/linux/desktops/archives/">здесь</a>
