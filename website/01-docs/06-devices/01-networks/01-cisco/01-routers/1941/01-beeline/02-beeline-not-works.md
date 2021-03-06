---
layout: page
title: Домашний интернет от Билайн, информация по неработающему интернету
description: Домашний интернет от Билайн, информация по неработающему интернету
keywords: devices, cisco, routers, 1941, Билайн, проблемы, не работает интернет, проблемы с брасом (dhcp)
permalink: /devices/cisco/routers/1941/beeline-not-works/
---

# Домашний интернет от Билайн, информация по неработающему интернету

Установлено оборудование Cisco Router 1941

<br/>

Учет ведется с помощью сервиса проверки доступности хоста из интернета. В случае обнаружения сервисом, что хост недоступен, он собирает информацию и предоставляет отчет, сколько времени хост был недоступен.

<br/>

**Последний раз аналогичная проблема:**

<br/>

**2021:**<br/>

08.04.2021 - 2 часа 18 минут

26.03.2021 - 1 час 08 минут

17.03.2021 - 2 часа 17 минут

01.03.2021 - 44 минуты

25.02.2021 - 1 час 23 минуты

29.01.2021 - 1 час 53 минуты

25.01.2021 - 29 минут + 7 минут

20.01.2021 - 3 часа, 41 минута

<br/>

**2020:**<br/>

25.04.2020 - 55 минут

11.04.2020 - 8 минут

10.04.2020 - 2 часа 40 минут

<br/>

**2019:**<br/>

26.10.2019 - 4 часа, 2 минуты

16.10.2019 - 3 часа, 18 минут

02.09.2019 - 2 часа 15 минут

02.04.2019 - 15 минут

05.03.2019 - Более 12 часов

<br/>

Более ранние записи не велись.

<br/>
<br/>

**Телефон поддержки:** <br/>
8-800-700-83-78

<br/>

Ошибка на стороне Beeline.

**Описание проблемы:**

Пропал доступ в интернет. Перезагрузил Cisco. Линк горит зеленым, но моргает не с должной интенсивностью. Консоль показывает что подключение по кабелю есть, но DHCP не отдает ip адрес. Покрутил бочку, повытаскивал из нее интерфейсы. Не помогло. Подключил к кабелю ноут. Тоже без результата.

Позвонил, робот сообщили об ошибке на участке, где я подключен. Робот пообещал отправить sms когда все починят. Ждемс.

<br/>

**Итого на чей стороне проблемы:**

2 (Я) : 18 (Beeline)

<br/>

+1 (Я)

Не открывался сайт. Было сообщение от яндекса, что "сайт содержит материалы для взрослых". Обратился в техподдержку. Думал beeline мне добавил "родительский контроль", "безопасный интернет", etc.

Оказалось, что у меня включен openvpn и мой сайт с клипами с ютуба, просто пытается использовать корпоративный маршрут, за который ни я, ни beeline не отвечаем. А уж там и настроены фильтры для сотрудников.

<br/>

### Что делаем в таких случаях

<br/>

```
// Чтобы подключиться с 20 ubuntu
$ ssh \
    -oKexAlgorithms=+diffie-hellman-group1-sha1 \
    -c aes256-cbc \
    192.168.1.1
```

<br/>

```
// Так можно подключиться с 18 ubuntu, с 20 уже нет.
$ ssh -c aes256-cbc 192.168.1.1
```

<br/>

```
cisco-router-1941> en
```

<br/>

```
# show ip interface brief
Interface                  IP-Address      OK? Method Status                Protocol
Embedded-Service-Engine0/0 unassigned      YES NVRAM  administratively down down
GigabitEthernet0/0         unassigned      YES DHCP   up                    up
GigabitEthernet0/1         192.168.1.1     YES NVRAM  up                    up
NVI0                       unassigned      YES unset  administratively down down
Virtual-PPP1               unassigned      YES NVRAM  administratively down down
```

<br/>

**DHCP билайновский не хочет отдавать мне мой IP.**

<br/>

```
# show dhcp lease
Temp IP addr: 0.0.0.0  for peer on Interface: GigabitEthernet0/0
Temp  sub net mask: 0.0.0.0
    DHCP Lease server: 0.0.0.0, state: 3 Selecting
    DHCP transaction id: 78
    Lease: 0 secs,  Renewal: 0 secs,  Rebind: 0 secs
    Next timer fires after: 00:00:01
    Retry count: 1   Client-ID: cisco-a493.4cba.00a0-Gi0/0
    Client-ID hex dump: 636973636F2D613439332E346362612E
                        303061302D4769302F30
    Hostname: cisco-router-1941
```

<br/>

**Буду интерфейсы перестартовывать**

<br/>

```
# conf t

# interface GigabitEthernet0/0
# shutdown

# no shutdown
```

<br/>

**Разумеется, не помогло!**
