---
layout: page
title: Docker в Windows
description: Docker в Windows
keywords: Docker в Windows
permalink: /server/windows/containers/docker/
---

# Docker в Windows

<br/>

[Docker в Windows первое знакомство](/server/windows/containers/docker/first-look/)

[Инсталляция Docker в Windows](/server/windows/containers/docker/installation/)

[Практический пример запуска приложения в docker под Windows](/server/windows/containers/docker/run-container/)

[Запустить docker контейнер и выполнить команду npm start](/server/windows/containers/docker/run-container-v2/)

[Запустить asp.net приложение в docker контейнере](/server/windows/containers/docker/run-asp-net-app-in-docker/)

<br/>

### Ошибки:

[Ошибки при работе с Docker в Windows](/server/windows/containers/docker/errors/)

<br/>

### Еще есть какой-то nanoserver от Microsoft.

https://docs.microsoft.com/en-us/virtualization/windowscontainers/quick-start/quick-start-windows-10

Но он у меня не запустился из коробки, а читать всю статью лень!

    $ docker pull microsoft/nanoserver
    $ docker run -it microsoft/nanoserver cmd
    $ powershell.exe Add-Content C:\helloworld.ps1 'Write-Host "Hello World"'

Оказывается, там есть какие-то переключатели между Windows и Linux контейнерами, Все это добро работает пока в режиме беты. Если что, можно полистать материалы следующего блога.

<br/>

<div align="center">
	<img src="//stefanscherer.github.io/content/images/2016/09/docker-for-windows-switch.gif" alt="Run Linux and Windows Containers on Windows 10" border="0" />
</div>

<br/>

https://stefanscherer.github.io/run-linux-and-windows-containers-on-windows-10/
