---
layout: page
title: Granting Docker Control to Non-root Users
permalink: /docker/installing-and-updating-docker/granting-docker-control-to-non-root-users/
---


## 04 Installing and Updating Docker

04 05 Granting Docker Control to Non-root Users


    $ sudo gpasswd -a <username> docker
    $ cat /etc/group
        docker:x:126:marley


    // Перелогиниваемся
    $logout

    $ docker run -it ubuntu /bin/bash
