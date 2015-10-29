---
layout: page
title: Инсталляция GIT в centos 6.x из исходников
permalink: /linux/dev/git/installation/centos/6/
---


### Инсталляция GIT в centos 6.x из исходников


Если git из стандартных репозиториев не устраивает. Например не пушит на github.


<h3>git 2.x</h3>


    # yum install -y git tar gcc

<br/>

    # yum install -y \
    curl-devel \
    expat-devel \
    gettext-devel \
    openssl-devel \
    zlib-devel

<br/>

    # yum install -y perl-ExtUtils-MakeMaker


============

    # cd /tmp

// Устанавливаем последнюю версию

    # git clone --depth=1 https://github.com/git/git.git

<br/>

    # cd git/
    $ cat GIT-VERSION-FILE

<br/>

    # mkdir -p /opt/git/2.6.0

<br/>

    # make prefix=/opt/git/2.6.0 all
    # make prefix=/opt/git/2.6.0 install

<br/>

    # yum remove -y git

<br/>

    $ /opt/git/2.6.0/bin/git --version

<br/>

    # su - developer

<br/>

    # vi ~/.bash_profile

<br/>

    #### GIT ##############################

        export GIT_HOME=/opt/git/2.6.2
        export PATH=$PATH:$GIT_HOME/bin

    #### GIT ##############################

<br/>

    $ source ~/.bash_profile

<br/>

    $ git --version



<br/><br/>

### Установка git в Ubuntu Linux из исходников


    $ sudo apt-get install -y git
    $ git --version
    $ cd /tmp
    $ git clone https://github.com/git/git.git
    $ cd git/
    $ less RelNotes
    Git v1.8.4

<br/>

    # sudo apt-get install -y \
    libcurl4-gnutls-dev \
    libexpat1-dev \
    gettext \
    libz-dev \
    libssl-dev \
    build-essential

<br/>

    $ make prefix=/opt/git/1.8.4 all
    $ sudo make prefix=/opt/git/1.8.4 install

<br/>

    $ sudo apt-get remove -y git

<br/>

    $ /opt/git/1.8.4/bin/git --version
    git version 1.8.3.1.437.g0dbd812

<br/>

    $ vi ~/.bash_profile

<br/>

    #### GIT ##############################

        export GIT_HOME=/opt/git/1.8.4
        export PATH=$PATH:$GIT_HOME/bin

    #### GIT ##############################

<br/>

    $ source ~/.bash_profile

<br/>

    $ git --version
    git version 1.8.3.1.437.g0dbd812



<br/>

### Некоторые команды:

Задать логин и эл.почту. Обязательно необходимо для коммитов.

    $ git config --global user.name "your_username"
    $ git config --global user.email "your_email"


Цвет в git

    $ git config --global color.ui true


Не отслеживать изменения chmod

    $ git config core.fileMode false


Посмотреть конфиг

    $ git config --list
    $ git config user.name
    $ git config user.email


Показывать текущую ветку (branch) в консоли:

    $ curl -o ~/.git-prompt.sh \
    https://raw.githubusercontent.com/gi.../git-prompt.sh

    $ source ~/.git-prompt.sh

    $ export PS1='[\W] git:$(__git_ps1 "(%s)") '

или

    $ export PS1='\W$(__git_ps1 "(%s)") > '

или

    $ export PS1='Geoff[\W]$(__git_ps1 "(%s)"): '
