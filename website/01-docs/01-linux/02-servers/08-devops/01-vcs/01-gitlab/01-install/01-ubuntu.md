---
layout: page
title: Инсталляция GITLAB в Ubuntu 18.04 из пакетов
permalink: /linux/servers/devops/vcs/gitlab/install/ubuntu/
---

# Инсталляция GITLAB в Ubuntu 18.04 из пакетов

Делаю:  
16.05.2019

<br/>

Предполагается что уже установлен <a href="/linux/servers/virtual/virtualbox/install/">VirtualBox</a>, <a href="/linux/servers/virtual/vagrant/install/ubuntu/">Vagrant</a>.

<br/>

    // Также предполагается, что установлен vagrant-hostmanager
    $ vagrant plugin install vagrant-hostmanager


<br/>

### Для начала в hosts пропишу


    # vi /etc/hosts

    192.168.0.11 gitlab.local


<br/>

### Приступаем к установке

    $ cd ~

    $ git clone https://bitbucket.org/sysadm-ru/vagrant.git

    $ cd vagrant/vagrantfiles/ubuntu18/


<br/>

Рекомендуется по меньшей мере 4GB оперативной памяти.
Делал с меньшим размером на виртуалке, получал ошибку на шаге:

    * bash[migrate gitlab-rails database] action run

С кодом ошибки, 137, о которой в интернетах пишут, что это код закончившейся оперативной памяти.

<br/>

    $ vi Vagrantfile 

Задаю размер памяти >= 4G

    v.memory = 4096


<br/>

    $ vagrant up

    $ vagrant ssh


<br/>

### В виртуальной машине

    $ sudo su -
    # apt install -y ca-certificates curl openssh-server postfix

PostFix Configuration --> Internet Site

    # curl -s https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | sudo bash

    # apt install -y gitlab-ce

<br/>

    # vi /etc/gitlab/gitlab.rb

    Указываю вместо:
        external_url 'http://gitlab.example.com'

    Свой ip:
        external_url 'http://gitlab.local'

<br/>

    # gitlab-ctl reconfigure && gitlab-ctl restart

<br/>

http://gitlab.local

<br/>

Поменять пароль.
Далее зайти под root с зданным паролем.

<br/>

# Gitlab runner (чтобы gitlab работал как CI/CD)


    # vi /etc/hosts

    127.0.0.1 gitlab.local

<br/>

    # apt install -y gitlab-runner

<br/>

Создаем любой проект. Заходим в него.


<!-- https://github.com/do-community/hello_hapi 


https://www.alibabacloud.com/blog/up-and-running-with-gitlab-ci%2Fcd-on-alibaba-cloud_594044

-->


<br/>

**Добавляю в корень проекта .gitlab-ci.yml:**

https://raw.githubusercontent.com/marley-nodejs/Learning-GitLab/master/Section%203/Video%203.1/.gitlab-ci.yml

<br/>

Удаляю строку  
image: busybox:latest

<br/>

Settings --> CI/CD --> Runners

Смотрим наши параметры, которые нужно будет задать в консоли. А именно gitlab url и gitlab-ci token.

<br/>

    $ sudo gitlab-runner register

    Please enter the gitlab-ci coordinator URL (e.g. https://gitlab.com/):
    [http://gitlab.local/][Enter] 

    Please enter the gitlab-ci token for this runner:
    [bCZh-V_zyksxUPipzYoB][Enter] 

    Please enter the gitlab-ci description for this runner:
    [This is gitlab CI/CD tutorial][Enter] 

    Please enter the gitlab-ci tags for this runner (comma separated):
    [stage,qu,build,deploy][Enter] 

    Whether to run untagged builds [true/false]:
    [true]: [Enter] 

    Whether to lock the Runner to current project [true/false]:
    [true]: [Enter]

    Please enter the executor: docker-ssh, virtualbox, docker+machine, docker, shell, ssh, docker-ssh+machine, kubernetes, parallels:
    [shell][Enter] 

    Runner registered successfully. Feel free to start it, but if it's running already the config should be automatically reloaded!

<br/>

Или как вариант (который в должной мере не тестировался)

```
$ sudo gitlab-runner register -n \
  --url http://gitlab.local/ \
  --registration-token bCZh-V_zyksxUPipzYoB \
  --executor shell \
  --description "shell-builder"
```



<br/>

Создается файл с конфигом раннера:

    /etc/gitlab-runner/config.toml



<!-- 
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

Settings --> CI/CD --> Runners  
Должен появиться созданный runner.

<br/>

Settings --> CI/CD --> Pipelines --> Run Pipeline

<br/>

При сообщении:

**the project doesn't have any runners online assigned to it.**

Можно, как вариант, в настройках runner указать галочку:

Run untagged jobs: Indicates whether this runner can pick jobs without tags 

<br/>


# Gitlab Registry (собственное хранилище docker контейнеров)

Поднимал на разных хостах gitlab и registry. При добавлении контейнера в registry все время было сообщение no **"container images stored for this project"**. Насколько понял из поисков. Чтобы работало, нужно, чтобы gitlab и registry были на одном хосте. 

<br/>

Сначала устанавливаем <a href="/linux/servers/containers/docker/install/ubuntu/">docker</a>

Потом поднимаем <a href="/linux/servers/containers/docker/self-hosted-registry/">docker registry</a>. Возможен вариант без TLS и с TLS.

<br/>

**Обращаю внимание, что на сервере с registry, необходимо сам сервер добавить в список разрешенных работы для клиента.** Если этого не сделать. То gitlab не запустится.

<br/>

### Установка

https://docs.docker.com/registry/insecure/


<br/>

https://docs.gitlab.com/ee/administration/container_registry.html


    # vi /etc/gitlab/gitlab.rb

<br/>


```
registry_external_url 'http://registry.local:5000'
```

<br/>

В случае использования версии с TLS, то должено быть соответственно:

registry_external_url 'https://registry.local'

А также нужно будет добавить (т.к. в конфиге не нашел) ссылки на сертификаты. См. подробности по ссылке на установку <a href="/linux/servers/containers/docker/self-hosted-registry/">registry</a>.

```
registry_nginx['ssl_certificate'] = "/home/vagrant/certs/selfsigned.crt"
registry_nginx['ssl_certificate_key'] = "/home/vagrant/certs/selfsigned.key"
```

<br/>

    # gitlab-ctl reconfigure && gitlab-ctl restart

<br/>

    # gitlab-ctl status
    # gitlab-ctl tail nginx

<br/>

<!-- 

    $ docker login registry.gitlab.local

-->


<!-- <br/>

    # cp /var/opt/gitlab/gitlab-rails/etc/gitlab.yml /var/opt/gitlab/gitlab-rails/etc/gitlab.yml.orig

    # vi /var/opt/gitlab/gitlab-rails/etc/gitlab.yml

<br/>

```
registry:
  enabled: true
  host: 192.168.1.11
  port: 5000
  api_url: http://localhost:5000/
```


<br/>

    # gitlab-ctl restart -->



<!-- https://www.digitalocean.com/community/tutorials/how-to-build-docker-images-and-host-a-docker-image-repository-with-gitlab

Создаем runner

```
sudo gitlab-runner register -n \
  --url https://gitlab.example.com/ \
  --registration-token your-token \
  --executor docker \
  --description "docker-builder" \
  --docker-image "docker:latest" \
  --docker-privileged
``` -->


<!-- 
// Запуск registry

https://docs.docker.com/registry/deploying/

<br/>





<br/> -->


<br/>

**По материалам:**  

<!--

Посмотри про переменные

https://stackoverflow.com/questions/38269701/using-a-private-docker-image-from-gitlab-registry-as-the-base-image-for-ci


Automatically build and push Docker images using GitLab CI
https://angristan.xyz/build-push-docker-images-gitlab-ci/

-->

https://www.youtube.com/watch?v=tMbG6RuI2dM

https://www.youtube.com/watch?v=fMdIH-L5Uvg&list=PLaFCDlD-mVOlnL0f9rl3jyOHNdHU--vlJ&index=4

https://www.youtube.com/watch?v=O5sbhykF-q4&list=PLaFCDlD-mVOlnL0f9rl3jyOHNdHU--vlJ&index=5