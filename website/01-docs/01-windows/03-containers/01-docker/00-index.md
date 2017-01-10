---
layout: page
title: Docker в Windows
permalink: /windows/containers/docker/
---

# Docker в Windows


<br/>

[Docker в Windows первое знакомство](/windows/containers/docker/first-look/)

[Инсталляция Docker в Windows](/windows/containers/docker/installation/)

[Практический пример запуска приложения в docker под Windows](/windows/containers/docker/run-container/)

[Запустить docker контейнер и выполнить команду npm start](/windows/containers/docker/run-container-v2/)

[Запустить asp.net приложение в docker контейнере](/windows/containers/docker/run-asp-net-app-in-docker/)




<br/>

### Ошибки:

[Ошибки при работе с Docker в Windows](/windows/containers/docker/errors/)


<br/>

### Еще есть какой-то nanoserver от Microsoft.


https://docs.microsoft.com/en-us/virtualization/windowscontainers/quick-start/quick-start-windows-10

Но он у меня не запустился из коробки, а читать всю статью лень!

    $ docker pull microsoft/nanoserver
    $ docker run -it microsoft/nanoserver cmd
    $ powershell.exe Add-Content C:\helloworld.ps1 'Write-Host "Hello World"'
