---
layout: page
title: Vagrant машины для Docker Swarm
description: Vagrant машины для Docker Swarm
keywords: Vagrant машины для Docker Swarm
permalink: /devops/linux/virtual/vagrant/for-docker-swarm/
---

# Vagrant машины для Docker Swarm

<br/>

    $ vagrant plugin install vagrant-hosts

<br/>

    $ vi Vagrantfile

```text

Vagrant.require_version ">= 1.9.1"


VAGRANTFILE_API_VERSION = "2"
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.boot_timeout = 900


  config.vm.provider :virtualbox do |v|
      # On VirtualBox, we don't have guest additions or a functional vboxsf
      # in CoreOS, so tell Vagrant that so it can be smarter.
      v.check_guest_additions = false
      v.functional_vboxsf     = false
    end


  config.vm.provision :hosts do |provisioner|

      provisioner.add_host '192.168.56.101', ['client']
      provisioner.add_host '192.168.56.102', ['ca']
      provisioner.add_host '192.168.56.103', ['manager1']
      provisioner.add_host '192.168.56.104', ['manager2']
      provisioner.add_host '192.168.56.105', ['manager3']
      provisioner.add_host '192.168.56.106', ['node1']
      provisioner.add_host '192.168.56.107', ['node2']
      provisioner.add_host '192.168.56.108', ['node3']

    end


  config.vm.define "client" do |myVm|

   myVm.ssh.insert_key = true
   #  myVm.ssh.forward_agent = true

    myVm.vm.box = 'ubuntu/xenial64'
    myVm.vm.hostname = 'client'

    myVm.vm.network :private_network, ip: "192.168.56.101"
    myVm.vm.network :forwarded_port, guest: 22, host: 10122, id: "ssh"


    myVm.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--memory", 512]
      v.customize ["modifyvm", :id, "--name", "client"]
    end
  end

  config.vm.define "ca" do |myVm|

    myVm.ssh.insert_key = true
    # myVm.ssh.forward_agent = true

    myVm.vm.box = 'ubuntu/xenial64'
    myVm.vm.hostname = 'ca'

    myVm.vm.network :private_network, ip: "192.168.56.102"
    myVm.vm.network :forwarded_port, guest: 22, host: 10222, id: "ssh"

    myVm.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--memory", 512]
      v.customize ["modifyvm", :id, "--name", "ca"]
    end
  end


  config.vm.define "manager1" do |myVm|

    myVm.ssh.insert_key = true
    # myVm.ssh.forward_agent = true


    myVm.vm.box = 'ubuntu/xenial64'

    myVm.vm.hostname = 'manager1'

    myVm.vm.network :private_network, ip: "192.168.56.103"
    myVm.vm.network :forwarded_port, guest: 22, host: 10322, id: "ssh"

    myVm.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--memory", 512]
      v.customize ["modifyvm", :id, "--name", "manager1"]
    end
  end


  config.vm.define "manager2" do |myVm|

    myVm.ssh.insert_key = true
    # myVm.ssh.forward_agent = true

    myVm.vm.box = 'ubuntu/xenial64'
    myVm.vm.hostname = 'manager2'

    myVm.vm.network :private_network, ip: "192.168.56.104"
    myVm.vm.network :forwarded_port, guest: 22, host: 10422, id: "ssh"

    myVm.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--memory", 512]
      v.customize ["modifyvm", :id, "--name", "manager2"]
    end
  end


  config.vm.define "manager3" do |myVm|

    myVm.ssh.insert_key = true
    # myVm.ssh.forward_agent = true

    myVm.vm.box = 'ubuntu/xenial64'
    myVm.vm.hostname = 'manager3'

    myVm.vm.network :private_network, ip: "192.168.56.105"
    myVm.vm.network :forwarded_port, guest: 22, host: 10522, id: "ssh"

    myVm.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--memory", 512]
      v.customize ["modifyvm", :id, "--name", "manager3"]
    end
  end


  config.vm.define "node1" do |myVm|

    myVm.ssh.insert_key = true
    # myVm.ssh.forward_agent = true

    myVm.vm.box = 'ubuntu/xenial64'

    myVm.vm.hostname = 'node1'

    myVm.vm.network :private_network, ip: "192.168.56.106"
    myVm.vm.network :forwarded_port, guest: 22, host: 10622, id: "ssh"

    myVm.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--memory", 512]
      v.customize ["modifyvm", :id, "--name", "node1"]
    end
  end


  config.vm.define "node2" do |myVm|

    myVm.ssh.insert_key = true
    # myVm.ssh.forward_agent = true

    myVm.vm.box = 'ubuntu/xenial64'

    myVm.vm.hostname = 'node2'

    myVm.vm.network :private_network, ip: "192.168.56.107"
    myVm.vm.network :forwarded_port, guest: 22, host: 10722, id: "ssh"

    myVm.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--memory", 512]
      v.customize ["modifyvm", :id, "--name", "node2"]
    end
  end


  config.vm.define "node3" do |myVm|

    myVm.ssh.insert_key = true
    # myVm.ssh.forward_agent = true

    myVm.vm.box = 'ubuntu/xenial64'
    myVm.vm.hostname = 'node3'

    myVm.vm.network :private_network, ip: "192.168.56.108"
    myVm.vm.network :forwarded_port, guest: 22, host: 10822, id: "ssh"

    myVm.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--memory", 512]
      v.customize ["modifyvm", :id, "--name", "node3"]
    end
  end

end

```

<br/>

    $ vagrant up

<br/>

// Если не стартовала а отвалилась по timeout, делаем следующее:

    $ vagrant halt node2
    $ vagrant up node2

    $ vagrant up node3

<br/>

    $ vagrant status
    Current machine states:

    client                    running (virtualbox)
    ca                        running (virtualbox)
    manager1                  running (virtualbox)
    manager2                  running (virtualbox)
    manager3                  running (virtualbox)
    node1                     running (virtualbox)
    node2                     running (virtualbox)
    node3                     running (virtualbox)
