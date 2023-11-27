# Исходники сайта [sysadm.ru](https://sysadm.ru)


<br/>

### Вариант для внесения изменений

Инсталлируете docker и docker-compose, далее:

```
$ cd ~
$ mkdir -p ~/projects/sysadm.ru && cd ~/projects/sysadm.ru
$ git clone --depth=1 https://github.com/webmakaka/sysadm.ru.git .
$ docker-compose up
```

<br/>

Остается в браузере подключиться к localhost:80


<br/>

### Запустить sysadm.ru на своем хосте с использованием docker контейнера:

```
$ docker run -i -t -p 80:80 --name sysadm.ru marley/sysadm.ru
```

<br/>

### Как сервис

```
# vi /etc/systemd/system/sysadm.ru.service
```

вставить содержимое файла sysadm.ru.service

```
# systemctl enable sysadm.ru.service
# systemctl start  sysadm.ru.service
# systemctl status sysadm.ru.service
```

http://localhost:4006
