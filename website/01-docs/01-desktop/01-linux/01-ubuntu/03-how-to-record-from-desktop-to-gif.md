---
layout: page
title: Запись с экрана монитора в GIF
description: Запись с экрана монитора в GIF
keywords: linux, ubuntu, gif, запись с экрана, peek
permalink: /desktop/linux/ubuntu/how-to-record-from-desktop-to-gif/
---

# Запись с экрана монитора в GIF

Делаю:  
2024.02.06

На Ubuntu 22.04.3 LTS у меня уже не работает.

Пишет ошибку:

```
Segmentation fault (core dumped)
```

Я так понял (из написанного в интернетах), что поддержку свернули.

Но появился https://github.com/firatkiral/pypeek

Норм запустился и записал кран.

<br/>

```
$ export PATH=$PATH:/home/marley/.local/bin
$ pypeek
```

<br/>

Делаю:  
07.03.2023

<br/>

```
$ sudo add-apt-repository ppa:peek-developers/stable
$ sudo apt-get update
$ sudo apt-get install -y peek

$ peek
```

<br/>

Если не удается найти на большом количестве мониторов, нужно в настройках монитров менять порядок, пока окно peek не появится. Наверное, стоит попробовать расположить в настройках дисплеи вертикально.

https://github.com/phw/peek/issues/512
