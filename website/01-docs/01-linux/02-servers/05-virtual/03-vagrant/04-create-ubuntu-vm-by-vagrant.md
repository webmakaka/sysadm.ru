---
layout: page
title: Создание с помощью Vagrant виртуальной машины Ubuntu
permalink: /linux/servers/virtual/vagrant/create-ubuntu-vm-by-vagrant/
---

# Создание с помощью Vagrant виртуальной машины Ubuntu

    $ mkdir -p ~/vagrant/ubuntu/
    $ cd ~/vagrant/ubuntu/

<br/>

    $ vi Vagrantfile

```shell

Vagrant.configure(2) do |config|

  config.vm.box = "ubuntu/bionic64"

  config.vm.provider "virtualbox" do |vb|
    #   # Display the VirtualBox GUI when booting the machine
    #   vb.gui = true
    #
    #   # Customize the amount of memory on the VM:
    vb.memory = "2048"
  end

end

```

    $ vagrant up
    $ vagrant ssh

<br/>

### Скрипт, чтобы стартовало с заданным ip адресом

Написано на коленке, наверняка, требует улучшений, упрощений и т.д.

```shell

VAGRANTFILE_API_VERSION = "2"
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.ssh.insert_key = false
  config.ssh.forward_agent = true

  config.vm.provision :hosts do |provisioner|

      provisioner.add_host '192.168.56.101', ['web']

    end

  config.vm.define "web" do |web|
    web.vm.box = 'ubuntu/bionic64'
    web.vm.hostname = 'web'

    web.vm.network :private_network, ip: "192.168.56.101"
    web.vm.network :forwarded_port, guest: 22, host: 10122, id: "ssh"

    web.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--memory", 2048]
      v.customize ["modifyvm", :id, "--name", "web"]
    end
  end

end

```

https://gist.github.com/maxivak/c318fd085231b9ab934e631401c876b1
