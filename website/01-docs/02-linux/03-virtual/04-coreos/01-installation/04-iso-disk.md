---
layout: page
title: Инсталляция CoreOS в virtualBox
permalink: /linux/virtual/coreos/installation/iso-disk/
---


### Первая попытка поставить на хост


Я сначала на хост поставил ubuntu, чтобы мне было проще. Ну там, чтобы конфиги редактировать, удаленно подключиться и т.д.

На хосте у меня уже сгенерирован публичный ключ, который нужно передать coreos.


Подключаюсь к ubuntu, которую я заменю coreos.


    # cd /tmp

    # wget https://raw.githubusercontent.com/coreos/init/master/bin/coreos-install
    # chmod +x coreos-install


    #  vi cloud-config.yaml

#cloud-config
ssh_authorized_keys:
  - ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCxOLyClFAUMRsdq73JsVgU5SnvOgEDXBKCy8g9Y/cMYKTiKfd3Zd2+jrdyDOLx8x/jIy8eKjFrthSomsqM8epf1JxPhtPQ0SEF/uIN87gr/PFINOnEmFc246kZfa8i015/b8piB9pSTmg9ND6i66yNNocBq/o4Sqb1zEEKuyDkn7wudmoMv+n2EXwzpoWn2QCJ4KjDSzpC6hoR+/cka1xM+rl+sPlU+G1kqgmdya7iwnyQg2e5SgxKJFK+ewvubX9gVTfzpUeo55L3yyftDd4a5JlUkCf9vJwJLH7r8OlLviqi8qr8v0lzm4AGjd6aZsQizFFYL5ZxpMLrNmGc3pyN

coreos:
    etcd:
      name: "core-01"
    units:
    - name: 00-eth0.network
      runtime: true
      content: |
        [Match]
        Name=eth1

        [Network]
        DNS=192.168.1.1
        Address=192.168.1.200/24
        Gateway=192.168.1.1



Проверка валидности конфига:  
https://coreos.com/validate/



    # wget https://github.com/sysadm-ru/coreos-cloud-config/blob/master/cloud-config.yaml
    # ./coreos-install -d /dev/sda -C stable -c ./cloud-config.yaml



Может быть полезным:  

https://deis.com/blog/2015/coreos-on-virtualbox  
https://coreos.com/os/docs/latest/installing-to-disk.html  
https://coreos.com/os/docs/latest/booting-with-iso.html  
http://www.youtube.com/watch?v=yiWa0KFJDfI  


http://www.liberidu.com/blog/2015/04/11/basic-newbie-install-coreos-on-virtualbox-getting-started-with-docker/
