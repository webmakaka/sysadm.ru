---
layout: page
title: Основные команды GIT
permalink: /linux/dev/git/commands/
---

# Основные команды GIT которые я использую

<br/>

### Глобальный конфиг

    $ vi ~/.gitconfig

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

### Не отслеживать изменения chmod

    $ git config core.fileMode false

<br/>

### git log

// Получить информацию по 2 последним коммитам

    $ git log -2

<br/>

// Получить лог удаленного (на другом хосте) репо

    $ git log origin/master

<br/>

// Посмотреть историю изменения одного файла

    $ git log -p имя_файла


<br/>

    $ git log --name-only

<br/>

// Получить дерево коммитов

    $ git log --graph --all --oneline --decorate -20


<br/>

###  Закоммитить все и отправить на сервер

    $ git status
    $ git add --all
    $ git commit -m "Что я сделал в результате работы"
    $ git push

-- отправить все бранчи на удаленный сервер

    $ git push --all -u


<br/>

### Показать все файлы которые могут быть добавлены в коммит.

    $ git status -u

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

###  Посмотреть что поменялось

-- посмотреть изменения в файле

    $ git diff website/01-docs/02-linux/08-containers/02-docker/00-index.md


-- посмотреть изменения только названия фалов

    $ git diff --name-only 06be2bf42c94c669f2c656593b10716fee7ad6dc

<br/>

###  Отменить сделанные изменения (все данные потеряются)

    $ git reset --hard


<br/>

###  Удалить untracked files

    $ git clean -fd

<br/>

###  Добавить изменения в мастер ветку

    $ git checkout master
    $ git merge --no-ff my_new_branch

--no-ff -- no fast forward    

<br/>

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

### Заменить заголовок коммита

-- Меняю в последнем коммите заголовок

    $ git commit --amend -m "ваш заголовок"

<br/>

### Поменять автора коммита

-- Меняю в последнем коммите автора

    $ git commit --amend --author "My Name <my-email@my-company.com>"

https://stackoverflow.com/questions/3042437/change-commit-author-at-one-specific-commit

<br/>

### both modified - выбрать одну из сторон, не делая ручной merge

If you're already in conflicted state, and you want to just accept all of theirs:

Принять их сторону

    git checkout --theirs .
    git add .

If you want to do the opposite:

Принять нашу сторону

    git checkout --ours .
    git add .

This is pretty drastic, so make sure you really want to wipe everything out like this before doing it.

https://stackoverflow.com/questions/10697463/resolve-git-merge-conflicts-in-favor-of-their-changes-during-a-pull


<br/>

### Выделение цветом

    $ git config --global color.ui true


Можно также посмотреть здесь:  
https://unix.stackexchange.com/questions/44266/how-to-colorize-output-of-git

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

### Для версиии 1.x сделать push как и в версии Git 2.0

-- я пользуюсь этим

  git config --global push.default simple

-- наоборот

  git config --global push.default matching



<br/>


### Откатить файлы к определенному коммиту

    git checkout a82de97faaafee458d47c60a51e12f7d7c7dba13 file_path/file_name

    git rebase -i HEAD~2