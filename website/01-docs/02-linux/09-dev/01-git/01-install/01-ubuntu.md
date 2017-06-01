---
layout: page
title: Инсталляция GIT из исходников в Ubuntu
permalink: /linux/dev/git/install/ubuntu/
---


# Инсталляция GIT из исходников в Ubuntu


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
