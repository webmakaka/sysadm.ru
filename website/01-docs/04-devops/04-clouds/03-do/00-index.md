---
layout: page
title: Digital Ocean (DO)
description: Digital Ocean (DO)
keywords: clouds, digital ocean
permalink: /devops/clouds/do/
---

# Digital Ocean (DO)

<br/>

## Terraform

<br/>

### [Webinar] Using Infrastructure as Code to Build Reproducible Systems with Terraform on DigitalOcean

<div align="center">
    <iframe width="853" height="480" src="https://www.youtube.com/embed/U5suIJwobiQ" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>

https://github.com/Zelgius/Infrastructure-As-Code-Intro

<br/>

### Terraform Digital Ocean

Наверное лучше для начала посмотреть вот этот материал:

<div align="center">
    <iframe width="853" height="480" src="https://www.youtube.com/embed/videoseries?list=PLtK75qxsQaMIHQOaDd0Zl_jOuu1m3vcWO" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>

https://github.com/groovemonkey/digitalocean-terraform

<br/>


Manage -> API -> Generate New Token

Token name: terraform-digitalocean

<br/>

Account --> Security --> ADD SSH KEY

<br/>

    $ mkdir ~/do-tf-project && cd ~/do-tf-project
    $ code .

**provider.tf**

```
variable "do_token" {}
variable "pub_key" {}
variable "pvt_key" {}
variable "ssh_fingerprint" {}

provider "digitalocean" {
    token = "${var.do_token}"
}

```

<br/>

**web1.tf**

```
resource "digitalocean_droplet" "web1" {
  image = "ubuntu-16-04-x64"
  name = "web1"
  region = "nyc2"
  size = "512mb"
  private_networking = true
  ssh_keys = [
    "${var.ssh_fingerprint}"
  ]
  connection {
    user = "root"
    type = "ssh"
    private_key = "${file(var.pvt_key)}"
    timeout = "2m"
  }
    provisioner "remote-exec" {
        inline = [
            "sudo apt-get update",
            "sudo apt-get install -y nginx"
        ]
    }
}
```

<br/>

    DO_TOKEN="TOKEN"
    SSH_FINGERPRINT="SSH_FINGERPRINT"

    $ terraform init

```
$ terraform plan \
-var "do_token"=${DO_TOKEN}" \
-var "pub_key"=${HOME}/.ssh/id_rsa.pub" \
-var "pvt_key"=${HOME}/.ssh/id_rsa" \
-var "ssh_fingerprint"=${SSH_FINGERPRINT}" \
```