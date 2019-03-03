---
layout: page
title: Деплой nodejs приложения на удаленный сервер с помощью ansible
permalink: /linux/servers/virtual/vagrant/deploy-node-app-by-ansible/
---

# Деплой nodejs приложения на удаленный сервер с помощью ansible

Делаю  
03.03.2019

<br/>

    $ vagrant plugin install vagrant-hosts

    $ vi Vagrantfile

<br/>

```shell

VAGRANTFILE_API_VERSION = "2"
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.ssh.insert_key = false
  config.ssh.forward_agent = true

  config.vm.provision :hosts do |provisioner|

      provisioner.add_host '192.168.56.101', ['client']
      provisioner.add_host '192.168.56.102', ['server']

  end

  config.vm.define "client" do |client|
    client.vm.box = 'ubuntu/bionic64'
    client.vm.hostname = 'client'

    client.vm.network :private_network, ip: "192.168.56.101"
    client.vm.network :forwarded_port, guest: 22, host: 10122, id: "ssh"

    client.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--memory", 512]
      v.customize ["modifyvm", :id, "--name", "client"]
    end

    client.vm.provision :shell, path: "bootstrap.sh"

  end

  config.vm.define "server" do |server|
    server.vm.box = 'ubuntu/bionic64'
    server.vm.hostname = 'server'

    server.vm.network :private_network, ip: "192.168.56.102"
    server.vm.network :forwarded_port, guest: 22, host: 10222, id: "ssh"

    server.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--memory", 512]
      v.customize ["modifyvm", :id, "--name", "server"]
    end
  end


  # config.vm.provision :shell, path: "bootstrap.sh"

end

```

<br/>

    $ vi bootstrap.sh

```
#!/usr/bin/env bash

apt update
apt install -y ansible

```

    $ ssh-add ~/.vagrant.d/insecure_private_key

    $ vagrant up

    $ vagrant status

<br/>

Двумя сессиями подключиться к созданным виртуальным машинам:

    $ vagrant ssh client
    $ vagrant ssh server

<br/>

### Client и Server

    $ sudo su  -
    # adduser --disabled-password --gecos "" user
    # usermod -aG sudo user
    # passwd user

    # vi /etc/ssh/sshd_config
    PasswordAuthentication yes

    # service sshd reload

<br/>

### Client

    # vi /etc/ansible/hosts

добавить ip или достаточно hostname

    server

<br/>

    # su - user

    $ ssh-keygen -t rsa
    $ ssh-copy-id user@server

    // Устанавливаем python 2.x на сервер
    $ ssh -tt server "sudo apt install -y python"

    $ cd ~
    $ git clone --depth=1 https://github.com/guoloi/cats_app_ansible
    $ cd cats_app_ansible/
    $ ansible-playbook main.yaml -K

http://192.168.56.102:8080/

Все хорошо
