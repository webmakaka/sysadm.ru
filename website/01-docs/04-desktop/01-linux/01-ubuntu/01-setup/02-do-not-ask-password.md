---
layout: page
title: Мелкие улучшения в Ubuntu
description: Мелкие улучшения в Ubuntu
keywords: Мелкие улучшения в Ubuntu
permalink: /desktop/linux/ubuntu/setup/do-not-ask-root-password/
---

# Мелкие улучшения в Ubuntu

<br/>

### Не спрашивать каждый раз пароль при комаде с sudo

    -- добавить при необходимости пользователя в группу sudo
    $ sudo usermod -aG sudo <username>

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

