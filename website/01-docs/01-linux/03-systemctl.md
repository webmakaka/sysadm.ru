---
layout: page
title: Команды запуска сервисов systemd (systemctl)
description: Команды запуска сервисов systemd (systemctl)
keywords: Команды запуска сервисов systemd (systemctl)
permalink: /linux/systemctl/
---

# Команды запуска сервисов systemd (systemctl)

<br/>

```
[Unit]
Description=MyApp
After=docker.service
Requires=docker.service

[Service]
TimeoutStartSec=0
ExecStartPre=-/usr/bin/docker kill %p
ExecStartPre=-/usr/bin/docker rm %p
ExecStartPre=/usr/bin/docker pull busybox
ExecStart=/usr/bin/docker run --name %p busybox /bin/sh -c "while true; do echo Hello World; sleep 1; done"
ExecStop=/usr/bin/docker stop %p

[Install]
WantedBy=multi-user.target
```

<br/>

    # systemctl enable /etc/systemd/system/hello.service

    # systemctl start /etc/systemd/system/hello.service

    # systemctl list-units

    # systemctl list-units hello.service

    # systemctl status hello.service

<br/>

    # systemctl daemon-reload

    # systemctl restart /etc/systemd/system/hello.service

<br/>
