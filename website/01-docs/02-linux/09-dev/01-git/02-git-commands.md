---
layout: page
title: Некоторые команды GIT
permalink: /linux/dev/git/commands/
---

# Некоторые команды GIT


// Задать парамеры идентификации git

    $ git config my_nick_name
    $ git config user.email "my_email@myEmail.ru"


<br/>

// Задать парамеры идентификации git глобально (лучше использовать локально, когда много git проектов с разными пользователями)

    $ git config --global my_nick_name
    $ git config --global user.email "my_email@myEmail.ru"


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

    $ git checkout -b my_new_branch


// Отправить этот бранч на удаленный git сервер

    $ git push -u origin my_new_branch


<br/>

###  Отменить сделанные изменения (все данные потеряются)

    $ git reset --hard


<br/>

###  Добавить изменения в мастер ветку

    $ git checkout master
    $ git merge my_new_branch

    // Удаление ненужной ветки
    $ git branch -D my_new_branch

<br/>

###


    $ git remote -v
    origin	https://github.com/sysadm-ru/sysadm.ru (fetch)
    origin	https://github.com/sysadm-ru/sysadm.ru (push)

<br/>

    $ git remote rm origin

<br/>

    $ git remote add origin https://sysadm-ru@bitbucket.org/sysadm-ru/sysadm.ru.git

    $ git push -u origin master
