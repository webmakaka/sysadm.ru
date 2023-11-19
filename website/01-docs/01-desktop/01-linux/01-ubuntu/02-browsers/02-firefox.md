---
layout: page
title: Инсталляция Firefox в Ubuntu
description: Инсталляция Firefox в Ubuntu
keywords: linux, ubuntu, firefox, browser, инсталляция
permalink: /desktop/linux/ubuntu/browsers/firefox/
---

# Инсталляция Firefox в Ubuntu

Делаю:  
2023.11.19

<br/>

Вот такое было на удаленной машине. Может и не лучшее решение. Но заработало.

<br/>

```
$  firefox
2023/11/19 17:56:18.690125 cmd_run.go:1055: WARNING: cannot start document portal: Expected portal at "/run/user/1000/doc", got "/home/marley/.cache/doc"
/system.slice/system-vncserver.slice/vncserver@1.service is not a snap cgroup
```

<br/>

Собственно, удалил firefox из snap и установил из apt репозитория.

<br/>

```
$ sudo snap remove firefox
```

<br/>

```
$ sudo add-apt-repository ppa:mozillateam/ppa
```

<br/>

```
echo '
Package: *
Pin: release o=LP-PPA-mozillateam
Pin-Priority: 1001
' | sudo tee /etc/apt/preferences.d/mozilla-firefox
```

<br/>

```
echo 'Unattended-Upgrade::Allowed-Origins:: "LP-PPA-mozillateam:${distro_codename}";' | sudo tee /etc/apt/apt.conf.d/51unattended-upgrades-firefox
```

<br/>

```
$ sudo apt install firefox
```

<br/>

https://www.omgubuntu.co.uk/2022/04/how-to-install-firefox-deb-apt-ubuntu-22-04
