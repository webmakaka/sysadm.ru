---
layout: page
title: Создание с помощью Vagrant 2-х виртуальных машин Debian
description: Создание с помощью Vagrant 2-х виртуальных машин Debian
keywords: Создание с помощью Vagrant 2-х виртуальных машин Debian
permalink: /virtual/vagrant/create-2-debian-vagrant/
---

# Создание с помощью Vagrant 2-х виртуальных машин Debian

<br/>

    $ vagrant plugin install vagrant-hosts

    $ vi Vagrantfile

```shell

VAGRANTFILE_API_VERSION = "2"
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.ssh.insert_key = false
  config.ssh.forward_agent = true

  config.vm.provision :hosts do |provisioner|

      provisioner.add_host '192.168.56.101', ['web']
      provisioner.add_host '192.168.56.102', ['db']

    end

  config.vm.define "web" do |web|
    web.vm.box = 'debian/jessie64'
    web.vm.hostname = 'web'

    web.vm.network :private_network, ip: "192.168.56.101"
    web.vm.network :forwarded_port, guest: 22, host: 10122, id: "ssh"

    web.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--memory", 512]
      v.customize ["modifyvm", :id, "--name", "web"]
    end
  end

  config.vm.define "db" do |db|
    db.vm.box = 'debian/jessie64'
    db.vm.hostname = 'db'

    db.vm.network :private_network, ip: "192.168.56.102"
    db.vm.network :forwarded_port, guest: 22, host: 10222, id: "ssh"

    db.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--memory", 512]
      v.customize ["modifyvm", :id, "--name", "db"]
    end
  end

end

```

    $ ssh-add ~/.vagrant.d/insecure_private_key

    $ vagrant up
