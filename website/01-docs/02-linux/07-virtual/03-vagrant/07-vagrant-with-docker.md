---
layout: page
title: Vagrant c Docker внутри
permalink: /linux/virtual/vagrant/vagrant-with-docker/
---


# Vagrant c Docker внутри


<br/>

    $ vi Vagrantfile


<br/>

{% highlight text %}

# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"
  config.vm.provision "docker"
end


{% endhighlight %}


<br/>

    $ vagrant box update
    $ vagrant up
    $ vagrant status

<br/>

    $ vagrant ssh default

<br/>

    $ docker -v
    Docker version 18.04.0-ce, build 3d479c0



<br/>

### Сразу с имиджем

{% highlight text %}

# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"
  config.vm.provision "docker" do |d|
   d.pull_images "node"
 end
end


{% endhighlight %}



<br/>

    $ vagrant ssh default

<br/>

    $ docker images
    REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
    node                latest              26cbfbc03e3f        2 days ago          675MB


<br/>





<br/>

### C docker compose


    $ vagrant plugin install vagrant-docker-compose
    
<br/>

{% highlight text %}

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"
  config.vm.hostname = 'vmsrv'
  config.vm.network "private_network", ip: "192.168.56.101"

 config.vm.provision "docker" do |d|
   d.pull_images "node"
 end
  config.vm.provision :docker_compose
end


{% endhighlight %}
