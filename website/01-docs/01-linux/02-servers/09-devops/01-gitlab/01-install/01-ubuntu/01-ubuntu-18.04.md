---
layout: page
title: Инсталляция GITLAB в Ubuntu 18.04 из пакетов
permalink: /linux/servers/devops/gitlab/install/ubuntu/18.04/
---

# Инсталляция GITLAB в Ubuntu 18.04 из пакетов

Делаю:  
17.03.2019

Рекомендуется по меньшей мере 4GB оперативной памяти.
Делал с меньшим размером на виртуалке, получал ошибку на шаге:

    * bash[migrate gitlab-rails database] action run

С кодом ошибки, 137, о которой в интернетах пишут, что это код закончившейся оперативной памяти.

<br/>

    $ sudo su -
    # apt install -y ca-certificates curl openssh-server postfix

Internet Site

    # curl -s https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | sudo bash
    # apt install -y gitlab-ce

<br/>

    # vi /etc/gitlab/gitlab.rb

    Указываю вместо:
        external_url 'http://gitlab.example.com'

    Свой ip:
        external_url 'http://192.168.56.101'

<br/>

    # gitlab-ctl reconfigure

<br/>

http://192.168.56.101

<br/>

Поменять пароль.
Далее закти под root с зданным паролем.

<br/>

**По материалам:**  
https://www.youtube.com/watch?v=tMbG6RuI2dM
