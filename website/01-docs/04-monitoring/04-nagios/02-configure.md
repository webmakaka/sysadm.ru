---
layout: page
title: Конфигурирование Nagios для мониторинга хостов
description: Конфигурирование Nagios для мониторинга хостов
keywords: adm, monitoring, nagios, ubuntu, configure
permalink: /monitoring/nagios/ubuntu/16.04/configure/
---

# Конфигурирование Nagios для мониторинга хостов

<br/>

    # cd /etc/nagios/objects/

    # vi contacts.cfg

    email                           nagios@localhost


    # cd ../
    # vi nagios.cfg



    # You can specify individual object config files as shown below:
    #cfg_file=/usr/local/nagios-4.3.1/etc/objects/commands.cfg
    #cfg_file=/usr/local/nagios-4.3.1/etc/objects/contacts.cfg
    #cfg_file=/usr/local/nagios-4.3.1/etc/objects/timeperiods.cfg
    #cfg_file=/usr/local/nagios-4.3.1/etc/objects/templates.cfg

    # Definitions for monitoring the local (Linux) host
    #cfg_file=/usr/local/nagios-4.3.1/etc/objects/localhost.cfg

<br/>

Оставляю:

    cfg_dir=/etc/nagios/objects



    # cd /etc/nagios/objects
    # mkdir ../disabled
    # mv switch.cfg windows.cfg printer.cfg ../disabled/

<br/>

Проверка конфига:

    # /usr/local/nagios/bin/nagios -v /etc/nagios/nagios.cfg


    # service nagios restart

Всякие конфиги

    # vi templates.cfg

<br/>

### Один хост

    # vi hosts.cfg

{% highlight text %}

define host {
use linux-server
host_name appserver1
alias appserver1
address 192.168.1.31
}

{% endhighlight %}

<br/>

    # vi services.cfg

{% highlight text %}

define service {
use generic-service
host appserver1
service_description HTTP
check_command check_http
}

{% endhighlight %}

    # service nagios restart

<br/>

### Несколько хостов

    # vi hosts.cfg

{% highlight text %}

define host {
use linux-server
host_name appserver1
alias appserver1
address appserver1
}

define host {
use linux-server
host_name appserver2
alias appserver2
address appserver2
}

{% endhighlight %}

<br/>

    # vi services.cfg

{% highlight text %}

define service {
use generic-service
host appserver1, appserver2
service_description HTTP
check_command check_http
}

{% endhighlight %}

    # service nagios restart

<br/>

### С использованием хостовых групп (hostgroups.cfg)

    # vi hostgroups.cfg

{% highlight text %}

define hostgroup {
hostgroup_name appservers
alias appservers
}

{% endhighlight %}

    # vi templates.cfg


    # Linux host definition template - This is NOT a real host, just a template!

{% highlight text %}

define host {
name appserver
use linux-server
register 0
hostgroups appservers
}

{% endhighlight %}

<br/>

    # vi services.cfg

{% highlight text %}

define service {
use generic-service
hostgroup_name appservers
service_description HTTP
check_command check_http
}

{% endhighlight %}
