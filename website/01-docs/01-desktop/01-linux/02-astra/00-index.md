---
layout: page
title: Подключаю debian репы в astra linux 1.8
description: Подключаю debian репы в astra linux 1.8
keywords: desktop, linux, astra, repos, debian, bookworm
permalink: /desktop/linux/astra/
---

# Astra Linux 1.8 (Debian 12/Bookworm)

<br/>

Делаю:  
2024.07.05

<br/>

### Подключаю debian репы в astra linux 1.8

<br/>

```
$ sudo vi /etc/apt/sources.list
```

<br/>

```
deb http://deb.debian.org/debian bookworm main non-free-firmware
deb-src http://deb.debian.org/debian bookworm main non-free-firmware

deb http://deb.debian.org/debian-security/ bookworm-security main non-free-firmware
deb-src http://deb.debian.org/debian-security/ bookworm-security main non-free-firmware

deb http://deb.debian.org/debian bookworm-updates main non-free-firmware
deb-src http://deb.debian.org/debian bookworm-updates main non-free-firmware
```

<br/>

```
// keyserver.ubuntu.com
$ gpg --keyserver keyring.debian.org --recv-keys 54404762BBB6E853 BDE6D2B9216EC7A8 E98404D386FA1D9 6ED0E7B82643E131

$ gpg --armor --export 54404762BBB6E853 | sudo apt-key add -
$ gpg --armor --export BDE6D2B9216EC7A8 | sudo apt-key add -
$ gpg --armor --export 0E98404D386FA1D9 | sudo apt-key add -
$ gpg --armor --export 6ED0E7B82643E131 | sudo apt-key add -
```

<br/>

```
$ sudo apt-get update -y
```

<br/>

```
$ sudo apt-get install -y locales-all
```
