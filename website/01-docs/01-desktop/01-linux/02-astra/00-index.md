---
layout: page
title: astra
description: astra
keywords: desktop, linux, astra
permalink: /desktop/linux/astra/
---

# Astra Linux 1.8 (Debian 12/Bookworm)

<br/>

Делаю:  
2024.06.27

<br/>

### Подключаю debian репы

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
$ sudo apt-get update -y
```

<br/>

```
$ gpg --keyserver pgp.mit.edu --recv-keys 0E98404D386FA1D9 6ED0E7B82643E131

$ gpg --armor --export 0E98404D386FA1D9 | sudo apt-key add -
$ gpg --armor --export 6ED0E7B82643E131 | sudo apt-key add -
```

```
$ sudo apt-get install -y locales-all
```
