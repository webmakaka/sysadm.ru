---
layout: page
title: Подготовка окружения для программирование в Linux на GO
permalink: /dev/go/env/
---

# Подготовка окружения для программирование в Linux на GO

**Делаю:**  
02.02.2019

<br/>

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

### Мой вариант инсталляции GO (в каталог /opt)

<br/>

    # cd /tmp/
    # wget --no-check-certificate https://redirector.gvt1.com/edgedl/go/go1.9.2.linux-amd64.tar.gz

<br/>

    # tar -xvzpf go1.9.2.linux-amd64.tar.gz
    # mkdir -p /opt/go/1.9.2
    # mv go/* /opt/go/1.9.2/
    # ln -s /opt/go/1.9.2 /opt/go/current

<br/>

    # su - ${username}

<br/>

    $  vi ~/.bashrc

<br/>

Добавляю строку в конец.

{% highlight shell linenos %}

###############################

# USER DEFINED

. ~/.bash_profile
###############################

{% endhighlight %}

<br/>

    $ vi ~/.bash_profile

<br/>

После

    # User specific environment and startup programs

<br/>

```
#### GO 1.9.2 ########################

    export GO_HOME=/opt/go/current
    export GOPATH=$HOME/go
    export PATH=${GO_HOME}/bin:$PATH

#######################################
```

<br/>

     $ source ~/.bash_profile

<br/>

    $ mkdir -p $GOPATH/src

<br/>

    $ go version
    go version go1.9.2 linux/amd64

<br/>

### Доп плагины для разработки на GO в Visual Studio Code

Rich Go Language support for Visual Studio
