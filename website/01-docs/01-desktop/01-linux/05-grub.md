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

<br/>

```
$ ./grub-customizer
```

<br/>

General settings -> default entry -> predefined -> Ubuntu with linux 5.11.0-27-generic

<br/>

### С помощью коммандной строки

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
