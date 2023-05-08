---
layout: page
title: Конвертировать wmdk в vdi
description: Конвертировать wmdk в vdi
keywords: Конвертировать wmdk в vdi
permalink: /virtual/virtualbox/convert-vmdk-vdi/
---

# Конвертировать wmdk в vdi

wmdk - vmware  
vdi - virtualbox

В Windows

    cd "C:\Program Files\Oracle\VirtualBox"

<br/>

    vboxmanage clonehd C:\Users\USER_NAME\Desktop\weblogic-deploy-scripts\wins-12.1.2-v5.43\wins-vbox09102013-disk1.vmdk C:\Users\USER_NAME\Desktop\weblogic-deploy-scripts\wins-12.1.2-v5.43\wins-vbox09102013-disk1.vdi --format VDI
