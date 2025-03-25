---
layout: page
title: Установка VSCODE в Ubuntu Linux
description: Установка VSCODE в Ubuntu Linux
keywords: desktop, linux, ubuntu, editors, vscode
permalink: /desktop/linux/ubuntu/editors/vscode/
---

<br/>

# Установка VSCODE в Ubuntu Linux

<br/>

```
$ curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
$ sudo install -o root -g root -m 644 microsoft.gpg /etc/apt/trusted.gpg.d/
$ sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main" > /etc/apt/sources.list.d/vscode.list'
```

<br/>

```
$ sudo apt install apt-transport-https
$ sudo apt update
$ sudo apt install -y code
```

<br/>

Дока:  
https://code.visualstudio.com/docs/setup/linux
