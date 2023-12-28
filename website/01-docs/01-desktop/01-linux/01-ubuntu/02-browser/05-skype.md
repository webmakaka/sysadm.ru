---
layout: page
title: Инсталляция Skype в Ubuntu 22.04
description: Инсталляция Skype в Ubuntu 22.04
keywords: desktop, linux, ubuntu, skype, инсталляция
permalink: /desktop/linux/ubuntu/skype/
---

# Инсталляция Skype в Ubuntu 22.04

<br/>

**Последний раз делаю:**  
2023.12.28


<br/>

На ноуте с процессором AMD, по правой кнопке не появляется меню, при этом подвисает skype. Skype установлен из SNAP. 

На форуме reddit у пользователей были аналогичные проблемы и они им предложили установить skype из deb пакета.


<br/>

```
// Удаляю skype, установленный ранее с помощью snap
$ snap remove skype
```

<br/>


```
// Прямая ссылка на deb пакет
$ wget https://go.skype.com/skypeforlinux-64.deb
$ sudo dpkg -i ./skypeforlinux-64.deb
```


<br/>

После переустановки стало работать