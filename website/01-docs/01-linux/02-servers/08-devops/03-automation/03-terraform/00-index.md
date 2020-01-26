---
layout: page
title: Terraform
permalink: /linux/servers/devops/automation/terraform/
---

# Terraform

<br/>

### [Install Terraform](/linux/servers/devops/automation/terraform/install/)

### [Terraform Google Cloud](/linux/servers/devops/automation/terraform/google-cloud/)


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