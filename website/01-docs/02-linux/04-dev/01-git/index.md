---
layout: page
title: Git из исходников
permalink: /linux/dev/git/
---



<h3>Git из исходников в centos 6.X</h3>


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



<br/><br/>
<hr/>
<br/><br/>

<strong>Установка git в Ubuntu Linux из исходников</strong>



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
