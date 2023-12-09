---
layout: page
title: Возникавшие ошибки при работе с VirtualBox
description: Возникавшие ошибки при работе с VirtualBox
keywords: Возникавшие ошибки при работе с VirtualBox
permalink: /server/linux/virtual/virtualbox/errors/
---

# Возникавшие ошибки при работе с VirtualBox

<br/>

### VirtualBox is configured with multiple host-only adapters with the same IP "192.168.56.1". Please remove one.

```shell
$ minikube start
Starting local Kubernetes v1.10.0 cluster...
Starting VM...
E1216 19:32:44.541503   10894 start.go:180] Error starting host: Error creating host: Error executing step: Running precreate checks.
: VirtualBox is configured with multiple host-only adapters with the same IP "192.168.56.1". Please remove one.

 Retrying.
E1216 19:32:44.541904   10894 start.go:186] Error starting host:  Error creating host: Error executing step: Running precreate checks.
: VirtualBox is configured with multiple host-only adapters with the same IP "192.168.56.1". Please remove one
================================================================================
An error has occurred. Would you like to opt in to sending anonymized crash
information to minikube to help prevent future errors?
To opt out of these messages, run the command:
	minikube config set WantReportErrorPrompt false
================================================================================
```

<br/>

    $ VBoxManage list -l hostonlyifs

    $ VBoxManage hostonlyif remove vboxnet1
