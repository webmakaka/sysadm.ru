---
layout: page
title: Подготовка окружения для программирование в Linux на GO
description: Подготовка окружения для программирование в Linux на GO
keywords: Подготовка окружения для программирование в Linux на GO
permalink: /dev/go/install/
---

# Подготовка окружения для программирование в Linux на GO

<br/>

### Мой вариант инсталляции GO (в каталог /opt)

<br/>

**Делаю:**  
23.11.2020

<br/>

    $ cd ~/tmp/
    $ wget --no-check-certificate https://golang.org/dl/go1.15.5.linux-amd64.tar.gz

<br/>

    $ tar -xvzpf go1.15.5.linux-amd64.tar.gz
    $ sudo mkdir -p /opt/go.1.15
    $ sudo mv go/* /opt/go.1.15/
    $ sudo ln -s /opt/go.1.15/ /opt/go

<br/>

    $ sudo vi /etc/profile.d/golang.sh

<br/>

```
#### GO 1.15 ########################

    export GO_HOME=/opt/go
    export PATH=${GO_HOME}/bin:$PATH

#### GO 1.15 ########################
```

    $ sudo chmod +x /etc/profile.d/golang.sh

<br/>

    $ source /etc/profile.d/golang.sh

<br/>

    $ go version
    go version go1.15.5 linux/amd64

<br/>

### Какие-то другие варианты

<div align="center">
    <iframe width="853" height="480" src="https://www.youtube.com/embed/9Pk7xAT_aCU" frameborder="0" allowfullscreen></iframe>
</div>

<br/>

https://gitlab.com/rvasily/msu-go-11/tree/master

<br/>

**Инсталляция:**

Вариант 1:

    $ sudo apt install golang

Вариант 2:

    # cd /tmp/
    # wget --no-check-certificate https://dl.google.com/go/go1.11.5.linux-amd64.tar.gz
    # tar -C /usr/local -xzf go1.11.5.linux-amd64.tar.gz

    # echo '
    ####################################
    export PATH=$PATH:/usr/local/go/bin
    export GOPATH=$HOME/GO
    export PATH=$PATH:$GOPATH/bin
    ####################################' >> /etc/profile

    # source /etc/profile

    # go version
    go version go1.11.5 linux/amd64

<br/>

### Пример компиляции из примера к видео

    # cd /tmp/
    # git clone https://gitlab.com/rvasily/msu-go-11
    # cd /tmp/msu-go-11/1/

    # go run ./0_hello/main.go
    Hello, World!

    # go build ./0_hello/main.go

// Получился main

    # ./main
    Hello, World!

<br/>

### Доп плагины для разработки на GO в Visual Studio Code

Rich Go Language support for Visual Studio
