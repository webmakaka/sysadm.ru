---
layout: page
title: Инсталляция GITLAB в Ubuntu 18.04 из пакетов
description: Инсталляция GITLAB в Ubuntu 18.04 из пакетов
keywords: Инсталляция GITLAB в Ubuntu 18.04 из пакетов
permalink: /dev/git/gitlab/install/ubuntu/
---

# Инсталляция GITLAB в Ubuntu 18.04 из пакетов

Делаю:  
18.05.2019

<br/>

Предполагается что уже установлен <a href="/devops/linux/virtual/virtualbox/install/">VirtualBox</a>, <a href="/devops/linux/virtual/vagrant/install/ubuntu/">Vagrant</a>.

<br/>

    // Также предполагается, что установлен vagrant-hostmanager
    $ vagrant plugin install vagrant-hostmanager

<br/>

### Для начала в hosts клиента пропишу

    # vi /etc/hosts

    127.0.0.1 gitlab.local

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

<br/>

    # vi /etc/hosts

    127.0.0.1 gitlab.local

<br/>

    # apt install -y ca-certificates curl openssh-server postfix

PostFix Configuration --> Internet Site

    # curl -s https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | sudo bash

    # apt install -y gitlab-ce gitlab-runner

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

# Gitlab Registry (собственное хранилище docker контейнеров)

### Не заработало!!! При запуске gitlab, он пытается использовать порт от docker registry. И не стартует.

    # gitlab-ctl tail nginx

```

2019-05-17_17:09:01.84880 2019/05/17 17:08:59 [emerg] 20123#0: bind() to 0.0.0.0:443 failed (98: Address already in use)
2019-05-17_17:09:02.35478 2019/05/17 17:08:59 [emerg] 20123#0: still could not bind()
2019-05-17_17:09:02.37749 2019/05/17 17:09:02 [emerg] 20128#0: bind() to 0.0.0.0:443 failed (98: Address already in use)
2019-05-17_17:09:02.87791 2019/05/17 17:09:02 [emerg] 20128#0: bind() to 0.0.0.0:443 failed (98: Address already in use)
2019-05-17_17:09:03.37821 2019/05/17 17:09:02 [emerg] 20128#0: bind() to 0.0.0.0:443 failed (98: Address already in use)
2019-05-17_17:09:03.88302 2019/05/17 17:09:02 [emerg] 20128#0: bind() to 0.0.0.0:443 failed (98: Address already in use)
2019-05-17_17:09:04.38324 2019/05/17 17:09:02 [emerg] 20128#0: bind() to 0.0.0.0:443 failed (98: Address already in use)
2019-05-17_17:09:04.88384 2019/05/17 17:09:02 [emerg] 20128#0: still could not bind()

```

<br/>

    // Получить информацию по установленной версии gitlab
    $ sudo gitlab-rake gitlab:env:info

```
System information
System:		Ubuntu 18.04
Current User:	git
Using RVM:	no
Ruby Version:	2.5.3p105
Gem Version:	2.7.6
Bundler Version:1.17.3
Rake Version:	12.3.2
Redis Version:	3.2.12
Git Version:	2.18.1
Sidekiq Version:5.2.5
Go Version:	unknown

GitLab information
Version:	11.10.4
Revision:	62c464651d2
Directory:	/opt/gitlab/embedded/service/gitlab-rails
DB Adapter:	PostgreSQL
DB Version:	9.6.11
URL:		http://gitlab.local
HTTP Clone URL:	http://gitlab.local/some-group/some-project.git
SSH Clone URL:	git@gitlab.local:some-group/some-project.git
Using LDAP:	no
Using Omniauth:	yes
Omniauth Providers:

GitLab Shell
Version:	9.0.0
Repository storage paths:
- default: 	/var/opt/gitlab/git-data/repositories
GitLab Shell path:		/opt/gitlab/embedded/service/gitlab-shell
Git:		/opt/gitlab/embedded/bin/git
```

<br/>

Жду предложение по решению проблемы. Сам устал искать решение.

<br/>

Поднимал на разных хостах gitlab и registry. При добавлении контейнера в registry все время было сообщение no **"container images stored for this project"**. Насколько понял из поисков,чтобы работало, нужно, чтобы gitlab и registry были на одном хосте.

<br/>

Сначала устанавливаем <a href="/devops/containers/docker/install/ubuntu/">docker</a>

<br/>

Потом поднимаем <a href="/devops/containers/docker/registry/">docker registry</a>. Возможен вариант без TLS и с TLS.

<br/>

И добавляем пользователя gitlab-runner:

    # usermod -aG docker gitlab-runner
    # service docker restart

<br/>

**Обращаю внимание, что на сервере с registry, необходимо сам сервер добавить в список разрешенных работы для клиента.**

<br/>

    # vi /etc/gitlab/gitlab.rb

<br/>

**Вариант без security**

```
registry_external_url 'http://registry.local:5000'

```

<br/>

**Вариант c tls security**

В случае использования версии с TLS, то должено быть соответственно:

registry_external_url 'https://registry.local'

<br/>

А также нужно будет добавить (т.к. в конфиге не нашел) ссылки на сертификаты. См. подробности по ссылке на установку <a href="/devops/containers/docker/registry/">registry</a>.

```
registry_nginx['ssl_certificate'] = "/home/vagrant/certs/selfsigned.crt"
registry_nginx['ssl_certificate_key'] = "/home/vagrant/certs/selfsigned.key"
```

<br/>

Далее:

    # gitlab-ctl reconfigure && gitlab-ctl restart

<br/>

    # gitlab-ctl status
    # gitlab-ctl tail nginx

<br/>

http://gitlab.local

<br/>

    // Должен loging проходить.
    # docker login gitlab.local

<br/>

Генерятся конфиги. В том числе для nginx:

    /var/opt/gitlab/nginx/conf/nginx.conf
    /var/opt/gitlab/nginx/conf/gitlab-http.conf
    /var/opt/gitlab/nginx/conf/gitlab-registry.conf

<!-- <br/>

    Если в hosts gitlab.local не 127.0.0.1 пытается коннектиться по https.

<br/>

    # /etc/hosts

    192.168.0.11 registry.local
    127.0.0.1 gitlab.local -->

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

### Пример Gitlab Runner shell executor без использования Docker

http://gitlab.local

Создаем любой проект. Заходим в него.

<!-- https://github.com/do-community/hello_hapi


https://www.alibabacloud.com/blog/up-and-running-with-gitlab-ci%2Fcd-on-alibaba-cloud_594044

-->

<br/>

**Добавляю в корень проекта .gitlab-ci.yml:**

https://raw.githubusercontent.com/webmakaka/Learning-GitLab/master/Section%203/Video%203.1/.gitlab-ci.yml

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

<!--
<br/>

### c Docker


```
$ sudo gitlab-runner register -n \
  --url http://gitlab.local/ \
  --registration-token nRBKy9zfE3Fagn2u6q3M \
  --executor shell \
  --description "shell_executor" \
  --tag-list "shell_executor"
```


CI/CD -- Pipelines -- Run

Run for: CI_CD_Using_SHELL_Executor

PASS -- MyPass --

-->

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
