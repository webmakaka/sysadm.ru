---
layout: page
title: Работа со снапшотами
permalink: /linux/virtual/virtualbox/snapshots/
---

# Работа со снапшотами


<br/>

### Создание снапшотов:

Созадем каталог для снапшотов для ранее созданной виртуальной машины.

    $ mkdir -p ${VM_HOME}/${vm}/snapshots

Определяем каталог для снапшотов (Виртуальная машина должна быть выключена)

    $ VBoxManage modifyvm ${vm} --snapshotfolder ${VM_HOME}/${vm}/snapshots

Создаем снапшот

    $ VBoxManage snapshot ${vm} take snapshot_name --description snapshot_description


<br/>

### Восстановление:

// Получаем информацию по интересующей нас виртуальной машине

    $ VBoxManage showvminfo ${vm}

....

Snapshots:

  Name: snapshot_name (UUID: 85e68d6b-bfc0-492d-8e05-e56aab773a33) *

    $ VBoxManage snapshot ${vm} restore 85e68d6b-bfc0-492d-8e05-e56aab773a33



    Restoring snapshot 85e68d6b-bfc0-492d-8e05-e56aab773a33
    0%...10%...20%...30%...40%...50%...60%...70%...80%...90%...100%


<br/>

### Удалить снапшот

    $ VBoxManage snapshot ${vm} delete 8728c91c-65b4-4b62-b7e6-34b5c8f02323
