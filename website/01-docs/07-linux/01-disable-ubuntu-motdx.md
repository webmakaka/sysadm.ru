---
layout: page
title: Linux - Disable Ubuntu motd spam
description: Linux - Disable Ubuntu motd spam
keywords: Linux - Disable Ubuntu motd spam
permalink: /linux/disable-ubuntu-motd/
---

# Disable Ubuntu motd spam

<br/>

Отключить на ubuntu server рекланые сообщения вроде:

<br/>

```
* Ubuntu's Kubernetes 1.14 distributions can bypass Docker and use containerd
directly, see https://bit.ly/ubuntu-containerd or try it now with
```

<br/>

```
$ sudo sed -i 's/^ENABLED=.*/ENABLED=0/' /etc/default/motd-news
```
