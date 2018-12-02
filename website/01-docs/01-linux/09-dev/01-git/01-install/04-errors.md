---
layout: page
title: Ошибки при работе с GIT
permalink: /linux/dev/git/errors/
---


# Ошибки при работе с GIT


<br/>

### fatal: HTTP request failed

    # git clone --depth=1 https://github.com/git/git.git
    Initialized empty Git repository in /tmp/git/.git/
    error:  while accessing https://github.com/git/git.git/info/refs

    fatal: HTTP request failed

<br/>

Исправилась выполнением команды:

    # yum update -y nss curl