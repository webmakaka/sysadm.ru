---
layout: page
title: Ubuntu - Выбор загружаемого при старте kernel в Ubuntu 20
description: Ubuntu - Выбор загружаемого при старте kernel в Ubuntu 20
keywords: Ubuntu - Выбор загружаемого при старте kernel в Ubuntu 20
permalink: /desktop/linux/ubuntu/setup/choose-kernel-on-startup/
---

# [Ubuntu] Выбор загружаемого при старте kernel в Ubuntu 20



<br/>

### Включить вывод приглашения выбора kernel при загрузке.

```
$ sudo vi /etc/default/grub
```

<br/>

```
GRUB_TIMEOUT_STYLE="hidden"
GRUB_TIMEOUT="0"
```

<br/>

```
#GRUB_TIMEOUT_STYLE="hidden"
GRUB_TIMEOUT="-1"
```

<br/>

```
$ sudo update-grub
```



<br/>

### GUI

<br/>

```
$ sudo apt install -y \
    grub-customizer
```


**Выпилено в версии 22.04**


Пишут, что можно установить следующим образом в 22.04

<br/>

https://itsubuntu.com/how-to-install-grub-customizer-on-ubuntu-22-04-lts/


<br/>

```
$ sudo add-apt-repository ppa:trebelnik-stefina/grub-customizer
$ sudo apt update
$ sudo apt install grub-customizer
```

<br/>

```
$ grub-customizer
```

<br/>

General settings -> default entry -> predefined -> Ubuntu with linux 5.13.0.35-generic
