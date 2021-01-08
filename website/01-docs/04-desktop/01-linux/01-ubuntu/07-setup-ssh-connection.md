---
layout: page
title: Подключение к серверу по SSH
description: Подключение к серверу по SSH
keywords: Подключение к серверу по SSH
permalink: /linux/ubuntu/setup-ssh-connection/
---

# Подключение к серверу по SSH


<br/>

### Подключение к серверу SSH с помощью rsa ключа (не спрашивая пароль)

    $ ssh-keygen -t rsa -b 4096
    $ cd ~/.ssh/

    $ ls
    authorized_keys  id_rsa.pub            known_hosts
    id_rsa           insecure_private_key  known_hosts.old

<br/>

    $ ssh-add
    $ ssh marley@192.168.1.5 mkdir .ssh
    $ cat id_rsa.pub | ssh marley@192.168.1.5 'cat >> .ssh/authorized_keys'
    $ ssh marley@192.168.1.5 'chmod 700 .ssh; chmod 640 .ssh/authorized_keys'
    $ ssh marley@192.168.1.5

<br/>

### Отключить доступ к серверу SSH по паролю (использовать rsa ключ)

    # vi /etc/ssh/sshd_config
    PasswordAuthentication no

<br/>

    # systemctl restart ssh
