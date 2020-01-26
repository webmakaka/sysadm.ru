---
layout: page
title: Install Terraform (Google cloud)
permalink: /linux/servers/devops/automation/terraform/install/
---

# Install Terraform (Google cloud)

Делаю:  
08.06.2019


<br/>

**0.11.14**

    $ wget https://releases.hashicorp.com/terraform/0.11.14/terraform_0.11.14_linux_amd64.zip

    $ unzip terraform_0.11.14_linux_amd64.zip

    $ sudo mv terraform /usr/local/bin/

    $ terraform version
    Terraform v0.11.14


<br/>

**0.12.20**

**Provider "google" v1.20.0 is not compatible with Terraform 0.12.20.**

    $ cd ~/tmp

    $ wget https://releases.hashicorp.com/terraform/0.12.20/terraform_0.12.20_linux_amd64.zip

    $ unzip terraform_0.12.20_linux_amd64.zip

    $ sudo mv terraform /usr/local/bin/

    $ terraform version
    Terraform v0.12.20
