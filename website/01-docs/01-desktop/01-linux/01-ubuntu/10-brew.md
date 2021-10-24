---
layout: page
title: Инсталляция менеджера пакетов Brew (HomeBrew, LinuxBrew) в Ubuntu
description: Инсталляция менеджера пакетов Brew (HomeBrew, LinuxBrew) в Ubuntu
keywords: brew, homebrew, linuxbrew
permalink: /desktop/linux/ubuntu/brew/
---

# Инсталляция менеджера пакетов Brew (HomeBrew, LinuxBrew) в Ubuntu

<br/>

```
// homebrew install
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

<br/>

```
$ sudo vi /etc/profile.d/brew.sh
```

<br/>

```
#### BREW #######################

export BREW_HOME=/home/linuxbrew/.linuxbrew/bin
export PATH=${BREW_HOME}/:$PATH

#### BREW #######################
```

<br/>

```
$ sudo chmod 755 /etc/profile.d/brew.sh
$ source /etc/profile.d/brew.sh
```
