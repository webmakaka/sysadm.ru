---
layout: page
title: Install Terraform (Google cloud)
permalink: /linux/servers/devops/automation/terraform/install/
---

# Install Terraform (Google cloud)

    $ wget https://releases.hashicorp.com/terraform/0.11.14/terraform_0.11.14_linux_amd64.zip

    $ unzip terraform_0.11.14_linux_amd64.zip

    $ sudo mv terraform /usr/local/bin/

    $ terraform version

<!--

    $ export PATH="$PATH:$HOME/terraform"
    $ cd /usr/bin
    $ sudo ln -s $HOME/terraform
    $ cd $HOME
    $ source ~/.bashrc

-->

