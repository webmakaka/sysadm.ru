---
layout: page
title: Ubuntu - не спрашивать каждый раз пароль при комаде с sudo
description: Ubuntu - не спрашивать каждый раз пароль при комаде с sudo
keywords: Ubuntu - не спрашивать каждый раз пароль при комаде с sudo
permalink: /desktop/linux/ubuntu/setup/do-not-ask-root-password/
---

# Ubuntu - не спрашивать каждый раз пароль при комаде с sudo

<br/>

    -- добавить при необходимости пользователя в группу sudo
    $ sudo usermod -aG sudo ${USER}

<br/>

    $ sudo su -

    # vi /etc/sudoers

<br/>

    %sudo   ALL=(ALL:ALL) ALL

меняю на:

```shell
#%sudo   ALL=(ALL:ALL) ALL
%sudo   ALL=(ALL:ALL) NOPASSWD:ALL
```

<!--
    root    ALL=(ALL:ALL) ALL

    меняю на

    root    ALL=(ALL:ALL) ALL
    <username>    ALL=(ALL:ALL) NOPASSWD:ALL -->

<br/>

    $ whoami
    marley

    $ sudo whoami
    root

