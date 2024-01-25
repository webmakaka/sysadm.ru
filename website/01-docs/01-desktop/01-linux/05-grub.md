---
layout: page
title: Выбор загружаемого при старте kernel в Ubuntu 20 с помощью GRUB
description: Выбор загружаемого при старте kernel в Ubuntu 20 с помощью GRUB
keywords: desktop, linux, ubuntu, setup, grub
permalink: /desktop/linux/grub/
---

# Выбор загружаемого при старте kernel в Ubuntu 20 с помощью GRUB

<br/>

### С помощью GUI

**Делаю: 2024.01.25**

<br/>

VirtualBox не запускался с последней версией. Пришлось искать постарше.

<br/>

```
$ sudo add-apt-repository ppa:danielrichter2007/grub-customizer
$ sudo apt-get update
$ sudo apt-get install -y grub-customizer

$ grub-customizer
```

<br/>

General settings -> default entry -> predefined -> Ubuntu with linux 6.5.0-14-generic

<br/>

### С помощью командной строки (Не заработало в последний раз!)

<br/>

```
$ sudo vi /etc/default/grub
```

<br/>

Включить вывод приглашения выбора kernel при загрузке.

<br/>

```
#GRUB_TIMEOUT_STYLE="hidden"
GRUB_TIMEOUT="-1"
```

<br/>

Отключить

<br/>

```
GRUB_TIMEOUT_STYLE="hidden"
GRUB_TIMEOUT="0"
```
