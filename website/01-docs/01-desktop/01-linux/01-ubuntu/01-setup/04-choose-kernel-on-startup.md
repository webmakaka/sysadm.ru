---
layout: page
title: Ubuntu - Выбор загружаемого при старте kernel в Ubuntu 20
description: Ubuntu - Выбор загружаемого при старте kernel в Ubuntu 20
keywords: Ubuntu - Выбор загружаемого при старте kernel в Ubuntu 20
permalink: /desktop/linux/ubuntu/setup/choose-kernel-on-startup/
---

# [Ubuntu] Выбор загружаемого при старте kernel в Ubuntu 20

<br/>

Были какие-то проблемы при старте. Пришлось выбрать kernel постарее.

<br/>

```
$ sudo apt install -y \
    grub-customizer
```

<br/>

```
$ grub-customizer
```

<br/>

General settings -> default entry -> predefined -> Ubuntu with linux 5.13.0.35-generic

<!--
5.11.0-27-generic

-->

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
