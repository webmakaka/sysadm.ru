---
layout: page
title: Terraform
description: Изучаем terraform
keywords: linux, terraform
permalink: /linux/devops/automation/terraform/
---

# Terraform


**Registry:**  
https://registry.terraform.io/


**Providers**
https://www.terraform.io/docs/providers/index.html


<br/>

### [Install Terraform](/linux/devops/automation/terraform/install/)

### [Terraform Google Cloud](/linux/devops/automation/terraform/google-cloud/)


```

$ docker pull hashicorp/terraform:latest
// $ docker pull hashicorp/terraform:0.12.20

$ docker run hashicorp/terraform --version
Terraform v0.12.20

$ mkdir ~/terraform 
$ cd ~/terraform/


$ vi main.tf

output "greeting" {
  value = "Hello Terraform!"
}

provider "random" {}


$ terraform init

$ terraform plan
$ terraform plan -out example.tfplan

$ terraform apply
$ teffaform apply example.tfplan


```

<br/>

# Install and configure tfswitch

```
$ wget https://github.com/warrensbox/terraform-switcher/releases/download/0.7.737/terraform-switcher_0.7.737_linux_amd64.tar.gz

$ mkdir -p ${HOME}/bin

$ tar -xvf terraform-switcher_0.7.737_linux_amd64.tar.gz -C ${HOME}/bin

$ export PATH=$PATH:${HOME}/bin

$ tfswitch -b ${HOME}/bin/terraform 0.11.14

$ echo "0.11.14" >> .tfswitchrc

$ exit

```