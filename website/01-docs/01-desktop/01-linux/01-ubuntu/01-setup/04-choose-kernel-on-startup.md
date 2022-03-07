---
layout: page
title: Ubuntu - Выбор kernel при загрузке
description: Ubuntu - Выбор kernel при загрузке
keywords: Ubuntu - Выбор kernel при загрузке
permalink: /desktop/linux/ubuntu/setup/choose-kernel-on-startup/
---

# [Ubuntu] Выбор kernel при загрузке


<br/>

Были какие-то проблемы при старте. Пришлось выбрать kernel постарее.


<br/>

```
$ sudo apt install -y \
    grub-customizer
```

<br/>

```
$ ./grub-customizer
```

<br/>

General settings -> default entry -> prdifined -> Ubuntu with linux 5.11.0-27-generic

<br/>

```
$ sudo vi /etc/default/grub
```

<br/>

```
GRUB_TIMEOUT_STYLE="hidden"
GRUB_TIMEOUT="0"
```

<br/>

Включить вывод приглашения выбора kernel при загрузке.

<br/>

```
#GRUB_TIMEOUT_STYLE="hidden"
GRUB_TIMEOUT="-1"
```
