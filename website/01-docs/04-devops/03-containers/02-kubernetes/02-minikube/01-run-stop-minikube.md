---
layout: page
title: Запуск и останов minikube
permalink: /devops/containers/kubernetes/minikube/run-stop-minikube/
---

# Запуск и останов minikube

Можно запускать с параметрами по умолчанию, не задавая никаких профилей.
Так даже проще.

## Запуск minikube

```
$ minikube --profile my-profile config set memory 4096
$ minikube --profile my-profile config set cpus 2


// Если нужно задать версию драйвера. Обычно не нужно. И так virtualbox по умолчанию
$ minikube --profile my-profile config set vm-driver virtualbox

// Если нужно задать определенную версию kubernetes
$ minikube --profile my-profile config set kubernetes-version v1.14.0
$ minikube --profile my-profile config set dashboard false
$ minikube start --profile my-profile

```

<br/>

```
$ minikube --profile my-profile config view
- dashboard: false
- ingress: true
- kubernetes-version: v1.14.0
- memory: 4096
- vm-driver: virtualbox
- cpus: 2

```

```
// Подключиться к виртуальной машине
$ minikube --profile my-profile ssh
```

```
$ minikube --profile my-profile ip
192.168.99.155
```

```
$ kubectl get events
$ kubectl get events --sort-by=.metadata.creationTimestamp
```

```
// Editor по умолчанию vscode
$ export KUBE_EDITOR="code -w"
```

```
$ minikube docker-env
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://192.168.99.159:2376"
export DOCKER_CERT_PATH="/home/marley/.minikube/certs"
# Run this command to configure your shell:
# eval $(minikube docker-env)
```

<br/>

## Останов minikube

    $ minikube --profile my-profile stop

    // Вообще все удалить
    $ minikube --profile my-profile delete

    $ minikube --profile my-profile stop && minikube --profile my-profile delete

<br/>

```
// Расположение профайлов
~/.minikube/profiles

// outputs the current profile
$ minikube profile

// lists all existing profiles
$ minikube profile list

```

<!--

    $ minikube profile default

```


minikube describe <profile>: outputs the JSON config of <profile>

minikube create <profile>: creates a new profile named, <profile>, and the associated cluster. It preserves all the flags supported by theminikube start command.

minikube delete <profile>: delete <profile>, the associated cluster, k8s context, and the ~/.minikube/profiles/<profile> subfolder.


```

-->

<br/>

    // Пересоздать все 1 командой
    $ minikube stop && minikube delete && minikube start

<br/>

**Дополнительно:**

https://github.com/burrsutter/9stepsawesome/
