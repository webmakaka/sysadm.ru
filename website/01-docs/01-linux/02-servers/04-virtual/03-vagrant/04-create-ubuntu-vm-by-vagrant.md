---
layout: page
title: Создание с помощью Vagrant виртуальной машины Ubuntu
permalink: /linux/servers/virtual/vagrant/create-ubuntu-vm-by-vagrant/
---


# Создание с помощью Vagrant виртуальной машины Ubuntu


    $ cd ~
    $ mkdir -p vagrant/ubuntu/

    $ cd vagrant/ubuntu/

    $ vi Vagrantfile


{% highlight text %}

Vagrant.configure(2) do |config|

  config.vm.box = "ubuntu/xenial64"

  config.vm.provider "virtualbox" do |vb|
    #   # Display the VirtualBox GUI when booting the machine
    #   vb.gui = true
    #
    #   # Customize the amount of memory on the VM:
    vb.memory = "2048"
  end


end

{% endhighlight %}


    $ vagrant up
    $ vagrant ssh




https://gist.github.com/maxivak/c318fd085231b9ab934e631401c876b1
