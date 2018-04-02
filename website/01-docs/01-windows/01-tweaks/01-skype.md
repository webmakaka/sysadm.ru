---
layout: page
title: Удалить рекламу из skype в Windows 10
permalink: /windows/tweaks/skype-do-not-show-advertising/
---

# Удалить рекламу из skype в Windows 10

Последний раз делал: 09.02.2018


1) Выгрузить из процессов skype.

2) Открыть файл хостс под администратором C:\Windows\System32\drivers\etc\hosts

И прописать в нем:

    127.0.0.1 rad.msn.com
    127.0.0.1 adriver.ru
    127.0.0.1 api.skype.com
    127.0.0.1 static.skypeassets.com
    127.0.0.1 apps.skype.com


3) Сделать копиию файла 

C:\Users\Имя_Пользователя\AppData\Roaming\Skype\Ник_в_Skype\config.xml

В оригинале в блоках 

    <AdvertPlaceholder>1</AdvertPlaceholder>
    <AdvertEastRailsEnabled>1</AdvertEastRailsEnabled>

Заменить 1 на 0.

4) Можно стартовать skype без рекламы.
