---
layout: page
title: Некоторые команды GIT
permalink: /linux/dev/git/commands/
---

# Некоторые команды GIT


// Задать парамеры идентификации git

    $ git config my_nick_name
    $ git config user.email "my_email@sysadm.ru"


<br/>

// Задать парамеры идентификации git глобально (лучше использовать локально, когда много git проектов с разными пользователями)

    $ git config --global my_nick_name
    $ git config --global user.email "my_email@sysadm.ru"


<br/>

// Посмотреть текущие параметры

    $ git config --list

    $ git config user.name
    $ git config user.email


<br/>

// Не отслеживать изменения chmod

    $ git config core.fileMode false



<br/>

// Получить информацию по 2 последним коммитам

    $ git log -2

<br/>

// Получить лог удаленного репо

    $ git log origin/master



<br/>

###  Закоммитить все и отправить на сервер

    $ git status
    $ git add --all
    $ git commit -m "Что я сделал в результате работы"
    $ git push

<br/>

###  Забрать git из удаленного репозитория (один из вариантов)

    $ git init

    $ git remote add origin http://myserver/myProject.git

    $ git pull

    $ git branch -a

    $ git checkout myProject_branch


<br/>

###  Создать новый бранч и отправить этот бранч на удаленный git сервер

// Создать новый бранч

    $ git checkout -b feature_SUZ-812


// Отправить этот бранч на удаленный git сервер

    $ git push -u origin feature_SUZ-812


<br/>

###  Отменить сделанные изменения (все данные потеряются)

    $ git reset --hard
