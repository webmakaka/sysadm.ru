---
layout: page
title: Основные команды GIT
permalink: /linux/dev/git/commands/
---

# Основные команды GIT


// Конфиг

    $ vi ~/.gitconfig


// Задать логин и эл.почту. (Обязательно необходимо для коммитов).

        $ git config --global user.name "your_username"
        $ git config --global user.email "your_email"

<br/>

// Задать парамеры идентификации git глобально (лучше использовать локально, когда много git проектов с разными пользователями)

    $ git config --global user.name "your_username"
    $ git config --global user.email "your_email"


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

// Иногда вот так приходится делать

    $ git push --set-upstream origin my_new_branch


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

### Заменить Remote Origin

    $ git remote -v
    origin	https://github.com/sysadm-ru/sysadm.ru (fetch)
    origin	https://github.com/sysadm-ru/sysadm.ru (push)

<br/>

    $ git remote rm origin

<br/>

    $ git remote add origin https://sysadm-ru@bitbucket.org/sysadm-ru/sysadm.ru.git

    $ git push -u origin master


<br/>

### Объединить несколько коммитов в 1 с помощью rebase

// Объединить 5 последних коммитов

    $ git rebase -i HEAD~5

заменить pick на squash для всех кроме первого.

Подробнее:

https://ru.stackoverflow.com/questions/462251/%D0%9A%D0%B0%D0%BA-%D0%BE%D0%B1%D1%8A%D0%B5%D0%B4%D0%B8%D0%BD%D0%B8%D1%82%D1%8C-%D0%BD%D0%B5%D1%81%D0%BA%D0%BE%D0%BB%D1%8C%D0%BA%D0%BE-%D0%BA%D0%BE%D0%BC%D0%BC%D0%B8%D1%82%D0%BE%D0%B2-%D0%B2-%D0%BE%D0%B4%D0%B8%D0%BD



<br/>

### Выделение цветом

    $ git config --global color.ui true

Можно также посмотреть здесь:  
https://unix.stackexchange.com/questions/44266/how-to-colorize-output-of-git


<br/>

### Не отслеживать изменения chmod

    $ git config core.fileMode false



<br/>

### Показывать текущую ветку (branch) в консоли:

    $ curl -o ~/.git-prompt.sh \
    https://raw.githubusercontent.com/git/git/master/contrib/completion/git-prompt.sh

    $ source ~/.git-prompt.sh

    $ export PS1='[\W] git:$(__git_ps1 "(%s)") '

или

    $ export PS1='\W$(__git_ps1 "(%s)") > '

или

    $ export PS1='Geoff[\W]$(__git_ps1 "(%s)"): '




<br/>

### both modified - выбрать одну из сторон, не делая ручной merge (пока изучаю)


If you're already in conflicted state, and you want to just accept all of theirs:

    git checkout --theirs .
    git add .

If you want to do the opposite:

    git checkout --ours .
    git add .

This is pretty drastic, so make sure you really want to wipe everything out like this before doing it.

https://stackoverflow.com/questions/10697463/resolve-git-merge-conflicts-in-favor-of-their-changes-during-a-pull
