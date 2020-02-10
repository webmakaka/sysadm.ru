---
layout: page
title: Terraform
description: Изучаем terraform
keywords: linux, terraform
permalink: /devops/automation/terraform/
---

# Terraform


**Registry:**  
https://registry.terraform.io/


**Providers**
https://www.terraform.io/docs/providers/index.html


<br/>

### [Install Terraform](/devops/automation/terraform/install/)

### [Terraform Google Cloud](/devops/automation/terraform/google-cloud/)


<br/>

### Основные команды

$ terraform init

$ terraform plan
$ terraform plan -out example.tfplan

$ terraform apply
$ teffaform apply example.tfplan


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

<br/>

### [[Webinar] Using Infrastructure as Code to Build Reproducible Systems with Terraform on DigitalOcean](/devops/clouds/do/terraform/)