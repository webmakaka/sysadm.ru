---
layout: page
title: Некоторые команды GIT
permalink: /linux/dev/git/commands/
---


### Некоторые команды GIT


// Задать парамеры идентификации git

    $ git config --global my_nick_name
    $ git config --global user.email "my_email@sysadm.ru"


// Посмотреть текущие параметры

    $ git config --list

    $ git config user.name
    $ git config user.email

// Не отслеживать изменения chmod

    $ git config core.fileMode false


// Получить лог удаленного репо

    $ git log origin11/master
