---
layout: page
title: SSH keys
permalink: /linux/virtual/vagrant/ssh-keygen/
---


# SSH keys


Create a new pair of SSH keys

    ssh-keygen -f /Users/MYUSER/.vagrant.d/insecure_private_key

Copy content of public key

    cat /Users/MYUSER/.vagrant.d/insecure_private_key.pub

On other shell in Homestead VM Machine copy into authorized_keys

    vagrant@homestad:~$ echo 'CONTENT_PASTE_OF_PRIVATE_KEY' >> ~/.ssh/authorized_keys

Now can access with vagrant ssh
