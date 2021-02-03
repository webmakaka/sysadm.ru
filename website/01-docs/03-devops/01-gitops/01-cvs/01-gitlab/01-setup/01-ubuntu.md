---
layout: page
title: Инсталляция GitLab в Ubuntu 20.04 из пакетов
description: Инсталляция GitLab в Ubuntu 20.04 из пакетов
keywords: devops, gitops, cvs, gitlab, setup, ubuntu
permalink: /devops/gitops/cvs/gitlab/setup/ubuntu/
---

# Инсталляция GitLab в Ubuntu 20.04 из пакетов

Делаю:  
02.02.2021

<br/>

Предполагается что уже установлен <a href="/devops/linux/virtual/virtualbox/setup/">VirtualBox</a>, <a href="/devops/linux/virtual/vagrant/setup/ubuntu/">Vagrant</a>.

<br/>

    // Также предполагается, что установлен vagrant-hostmanager
    $ vagrant plugin install vagrant-hostmanager

<br/>

### Для начала в hosts клиента пропишу

<br/>

```
$ sudo vi /etc/hosts

192.168.0.5 gitlab.local
```

<br/>

## Поднимаю в виртуалке GitLab

    $ mkdir ~/vagrant-gitlab && cd ~/vagrant-gitlab

<br/>

**Создаю Vagrantfile для виртуалки**

<br/>

Рекомендуется по меньшей мере 4GB оперативной памяти.
Делал с меньшим размером на виртуалке, получал ошибку на шаге:

    * bash[migrate gitlab-rails database] action run

С кодом ошибки, 137, о которой в интернетах пишут, что это код закончившейся оперативной памяти.

<br/>

C 4GB зависала. Накинул еще 1GB.

<br/>

```
$ cat <<EOF >> Vagrantfile

# -*- mode: ruby -*-
# vi: set ft=ruby :

ENV['VAGRANT_NO_PARALLEL'] = 'yes'

Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/focal64"
  config.hostmanager.enabled = true
  config.hostmanager.include_offline = true

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "5120"
    vb.cpus = 2
  end

  config.vm.define "gitlab.local" do |c|
    c.vm.hostname = "gitlab.local"
    c.vm.network "private_network", ip: "192.168.0.5"
  end
end
EOF
```

<br/>

    $ vagrant box update
    $ vagrant up

<br/>

    $ vagrant ssh gitlab.local

<br/>

Внутри виртуальной машины:

<br/>

    $ sudo apt update && sudo apt upgrade -y

<br/>

    $ sudo apt install -y \
        openssh-server \
        rar unrar-free \
        unzip

<br/>

**Создаю пользователя "gitlab"**

    $ sudo su  -
    # adduser --disabled-password --gecos "" gitlab
    # usermod -aG sudo gitlab
    # passwd gitlab

<br/>

**Предоставляю возможность подключения по SSH**

    # sed -i "s/.*PasswordAuthentication.*/PasswordAuthentication yes/g" /etc/ssh/sshd_config

    # service sshd reload

<br/>

**Разрешаю выполнение команд sudo без пароля**

<br/>

    # vi /etc/sudoers

<br/>

    %sudo ALL=(ALL:ALL) ALL

меняю на:

```shell
#%sudo   ALL=(ALL:ALL) ALL
%sudo   ALL=(ALL:ALL) NOPASSWD:ALL
```

<br/>

Убедиться, что прописан gitlab.local

```
# vi /etc/hosts
192.168.0.5 gitlab.local
```

<br/>

    # apt install -y ca-certificates curl openssh-server postfix

<br/>

PostFix Configuration --> Internet Site

<br/>

**Чтобы какой-то пакет брался из репо gitlab а не репо ubuntu**

```
# cat <<EOF | sudo tee /etc/apt/preferences.d/pin-gitlab-runner.pref
Explanation: Prefer GitLab provided packages over the Debian native ones
Package: gitlab-runner
Pin: origin packages.gitlab.com
Pin-Priority: 1001
EOF
```

<br/>

**Подключаю оф. репозитории**

```
    # curl -s https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | sudo bash

    # curl -L "https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh" | sudo bash
```

<br/>

```
    # export GITLAB_RUNNER_DISABLE_SKEL=true

    # apt install -y gitlab-ce gitlab-runner
```

<br/>

```
# vi /etc/gitlab/gitlab.rb

Указываю вместо:
    external_url 'http://gitlab.example.com'

Свой hostname:
    external_url 'http://gitlab.local'
```

<br/>

    # gitlab-ctl reconfigure && gitlab-ctl restart

<br/>

http://gitlab.local

Поменять пароль для пользователя root.
Далее зайти под root с зданным паролем.

<br/>

## Попытка запустить какую-нибудь задачу

    $ su - gitlab

<br/>

Для начала, установлю <a href="/devops/containers/docker/setup/ubuntu/">docker</a>

    $ sudo usermod -aG docker gitlab
    $ sudo usermod -aG docker vagrant
    $ sudo usermod -aG docker gitlab-runner

<br/>

```
$ gitlab-runner verify
```

<br/>

Создать нового пользователя и залогиниться им.

(Root в админке (Admin area) должен разрешить работу с gitlab созданным юзером.)

<br/>

Генерю rsa ключи:

    $ ssh-keygen -t rsa -N ''
    $ cat ~/.ssh/id_rsa.pub

<br/>

Settings --> SSH Keys

Добавить ключ с хост машины.

<br/>

Импортирую репо (New Project -> Import -> Repo Url):  
https://bitbucket.org/marley-golang/continuous-integration-on-gitlab/

<br/>

Войти как root --> Admin area --> Runners --> Скопировать registration token

<br/>

    $ sudo gitlab-runner register

<br/>

```
Please enter the gitlab-ci coordinator URL: [http://gitlab.local]
Please enter the gitlab-ci token for this runner: [<myToken>]
Please enter the gitlab-ci description for this runner: [local-docker-runner]
Please enter the gitlab-ci tags for this runner (comma separated): [go-runner]
Please enter the executor: [docker]
Default docker image: [golang:1.7]
```

<br/>

```
Runner registered successfully. Feel free to start it, but if it's running already the config should be automatically reloaded!
```

<br/>

    $ sudo vi /etc/gitlab-runner/config.toml

В конец блока:

    [runner.docker]

добавить

    extra_hosts = ["gitlab.local:192.168.0.5"]

<br/>

    $ sudo gitlab-runner restart

<br/>

Изменить в файле проекта, например, в файле main.go текст World на что-нибудь.

CI/CD -> Pipelines

Все OK.

<!--
<br/>
    $ sudo gitlab-runner run
-->

<br/>

### Ошибки

Была ошибка:

```
ERRO[0000] Docker executor: prebuilt image helpers will be loaded from /var/lib/gitlab-runner.
Running in system-mode.
```

Пришлось переустановить runner. Ранее у меня был установлен из репозитория ubuntu.
Установил из оф.репо. Заработало.

<br/>

### Полезные команды:

<br/>

    // Получить информацию по установленной версии gitlab
    $ sudo gitlab-rake gitlab:env:info

<br/>

    // логи nginx
    # gitlab-ctl tail nginx

<!--

```
$ sudo gitlab-runner register -n \
  --url http://gitlab.local/ \
  --registration-token bCZh-V_zyksxUPipzYoB \
  --executor shell \
  --description "shell-builder"
```

```
sudo gitlab-runner register -n \
  --url http://gitlab.local/ \
  --registration-token bCZh-V_zyksxUPipzYoB \
  --executor docker \
  --description "docker-builder" \
  --docker-image "docker:latest" \
  --docker-privileged
```

-->

<br/>

### Может быть полезно

**Install GitLab Runner using the official GitLab repositories**  
https://docs.gitlab.com/runner/install/linux-repository.html
