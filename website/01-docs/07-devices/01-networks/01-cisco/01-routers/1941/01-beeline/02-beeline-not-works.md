---
layout: page
title: Cisco Router 1941 не работает интернет в локальной сети Билайн
description: Cisco Router 1941 не работает интернет в локальной сети Билай
keywords: Cisco Router 1941, Билайн, проблемы с интернетом, dhcp
permalink: /devices/cisco/routers/1941/beeline-not-works/
---

# Cisco Router 1941 не работает интернет в локальной сети Билайн

<br/>

**Последний раз аналогичная проблема:**

<br/>

<br/>

**2021:**<br/>

29.01.2021 - 1 hour and 53 minutes

25.01.2021 - 29 + 7 minutes

20.01.2021 - 3 hours, 41 minutes

<br/>

**2020:**<br/>

25.04.2020 - 55 minutes

11.04.2020 - 8 minutes

10.04.2020 - 2 часа 40 минут

<br/>

**2019:**<br/>

26.10.2019 - 4 часа, 2 минуты

16.10.2019 - 3 часа, 18 минут

02.09.2019 - 2 часа 15 минут

02.04.2019 - 15 минут

05.03.2019 - Более 12 часов

<br/>
<br/>

**Телефон поддержки:** <br/>
8-800-700-83-78

<br/>

Ошибка на стороне Beeline.

**Описание проблемы:**

Пропал доступ в интернет. Перезагрузил Cisco. Линк горит зеленым, но морганет не с должной интенсивностью. Консоль показывает что подключение по кабелю есть, но DHCP не отдает ip адрес. Покрутил бочку, повытаскивал из нее интерфейсы. Не помогло. Подключил к кабелю ноут. Тоже без результата.

Позвонил, робот сообщили об ошибке на участке, где я подключен. Робот пообещал отправить sms когда все починят. Ждемс.

<br/>

**Итого на чей стороне проблемы:**

2 (Me) : 13 (Beeline)

<br/>

### Что делаем в таких случаях

    $ ssh -c aes256-cbc 192.168.1.1

    cisco-router-1941> en

<br/>

    # show ip interface brief
    Interface                  IP-Address      OK? Method Status                Protocol
    Embedded-Service-Engine0/0 unassigned      YES NVRAM  administratively down down
    GigabitEthernet0/0         unassigned      YES DHCP   up                    up
    GigabitEthernet0/1         192.168.1.1     YES NVRAM  up                    up
    NVI0                       unassigned      YES unset  administratively down down
    Virtual-PPP1               unassigned      YES NVRAM  administratively down down

<br/>

    DHCP билайновский не хочет отдавать мне IP.

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

    Буду интерфейсы перестартовывать.

<br/>

    # conf t

    # interface GigabitEthernet0/0
    # shutdown

    # no shutdown
