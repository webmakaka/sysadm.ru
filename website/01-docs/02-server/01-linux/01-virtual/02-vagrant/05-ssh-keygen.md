---
layout: page
title: SSH keys
description: SSH keys
keywords: SSH keys
permalink: /server/linux/virtual/vagrant/ssh-keygen/
---

# SSH keys (По прошествию времени, у меня по поводу всего этого какие-то сомнения)

Create a new pair of SSH keys

    $ ssh-keygen -f ~/.vagrant.d/insecure_private_key

Copy content of public key

    $ cat ~/.vagrant.d/insecure_private_key.pub

On other shell in Homestead VM Machine copy into authorized_keys

    $ echo 'CONTENT_PASTE_OF_PRIVATE_KEY' >> ~/.ssh/authorized_keys

Now can access with vagrant ssh
