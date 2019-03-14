---
layout: page
title: Jenkins
permalink: /linux/servers/devops/jenkins/
---

# Jenkins

### Запуск в docker контейнере

**jenkins.sh**

```
#!/bin/bash
docker run -p 8080:8080 -p 50000:50000 jenkins/jenkins
```

<br/>

    // Получить пароль
    $ docker exec <container_id> cat /var/jenkins_home/secrets/initialAdminPassword
