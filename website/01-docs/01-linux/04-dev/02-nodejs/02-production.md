---
layout: page
title: How To Set Up a Node.js Application for Production on Ubuntu 16.04
permalink: /linux/dev/nodejs/production/
---

# How To Set Up a Node.js Application for Production on Ubuntu 16.04

with PM2 & Nginx

Взято:

https://gist.github.com/tomysmile/9c329a40dbd7da917d52a4886fd32fc3

https://gist.githubusercontent.com/andrIvash/52a175768b90ea21df11d609c0ba9ad2/raw/a19bb5d3dbd1d6fbc16a7f5593adb06e2abfa7d4/node-setup-pm2-nginx.md

P.S. Для себя делаю в контейнерах docker.

<br/>

## Create User

as a root, run below commands on server:

```
# adduser tomy
# usermod -aG sudo tomy
```

### Generate Keygen

on your local machine (osx), runs below:

```
$ ssh-keygen
```

Assuming your local user is called "localuser", you will see output like this:

```
ssh-keygen output
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/localuser/.ssh/id_rsa):
```

### Copy the public key

#### Option 1: Use ssh-copy-id

If your local machine has the ssh-copy-id script installed, you can use it to install your public key to any user that you have login credentials for.

Run the ssh-copy-id script by specifying the user and IP address of the server that you want to install the key on, like this:

```
ssh-copy-id tomy@SERVER_IP_ADDRESS
```

After providing your password at the prompt, your public key will be added to the remote user's .ssh/authorized_keys file. The corresponding private key can now be used to log into the server.

#### Option 2: Manually Install the Key

Assuming you generated an SSH key pair using the previous step, use the following command at the terminal of your local machine to print your public key (id_rsa.pub):

```
cat ~/.ssh/id_rsa.pub
```

Copy the content to your clipboard.

To enable the use of SSH key to authenticate as the new remote user, you must add the public key to a special file in the user's home directory.

On the server, as the root user, enter the following command to temporarily switch to the new user (substitute your own user name):

```
su - tomy
```

Now you will be in your new user's home directory.

Create a new directory called .ssh and restrict its permissions with the following commands:

```
mkdir ~/.ssh
chmod 700 ~/.ssh
```

Now open a file in .ssh called authorized_keys with a text editor. We will use nano to edit the file:

```
nano ~/.ssh/authorized_keys
```

Now insert your public key (which should be in your clipboard) by pasting it into the editor.

Hit CTRL-x to exit the file, then y to save the changes that you made, then ENTER to confirm the file name.

Now restrict the permissions of the authorized_keys file with this command:

```
chmod 600 ~/.ssh/authorized_keys
```

Type this command once to return to the root user:

```
exit
```

Now your public key is installed, and you can use SSH keys to log in as your user.

## Install Node.js

runs below as root (see step no. 1 above):

```
$ sudo apt-get update & apt-get upgrade
$ sudo apt-get install libkrb5-dev build-essential
$ curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash -
$ sudo apt-get install -y nodejs
```

## Install PM2

runs below as root :

```
$ sudo npm install pm2 -g --unsafe-perm
```

as 'safe user', now we have to create PM2 startup service:

```
$ sudo su -c "env PATH=$PATH:/usr/bin pm2 startup systemd -u tomy --hp /home/tomy"
```

This will create a systemd unit which runs pm2 for your user on boot. This pm2 instance, in turn, runs hello.js or any script or apps you have defined. You can check the status of the systemd unit with systemctl:

```
$ systemctl status pm2
```

### Other PM2 Usage (Optional)

Stop an application with this command (specify the PM2 App name or id):

```
$ pm2 stop app_name_or_id
```

Restart an application with this command (specify the PM2 App name or id):

```
$ pm2 restart app_name_or_id
```

The list of applications currently managed by PM2 can also be looked up with the list subcommand:

```
$ pm2 list
```

More information about a specific application can be found by using the info subcommand (specify the PM2 App name or id):

```
$ pm2 info example
```

The PM2 process monitor can be pulled up with the monit subcommand. This displays the application status, CPU, and memory usage:

```
$ pm2 monit
```

## Install Nginx

```
$ sudo apt-get install nginx
```

### Adjust Firewall

We can list the applications configurations that ufw knows how to work with by typing:

```
$ sudo ufw app list
```

You should get a listing of the application profiles:

```
Output
Available applications:
  Nginx Full
  Nginx HTTP
  Nginx HTTPS
  OpenSSH
```

As you can see, there are three profiles available for Nginx:

-   Nginx Full: This profile opens both port 80 (normal, unencrypted web traffic) and port 443 (TLS/SSL encrypted traffic)

-   Nginx HTTP: This profile opens only port 80 (normal, unencrypted web traffic)

-   Nginx HTTPS: This profile opens only port 443 (TLS/SSL encrypted traffic) It is recommended that you enable the most restrictive profile that will still allow the traffic you've configured. Since we haven't configured SSL for our server yet, in this guide, we will only need to allow traffic on port 80.

You can enable this by typing:

```
$ sudo ufw allow 'Nginx HTTP'
```

You can verify the change by typing:

```
$ sudo ufw status
```

You should see HTTP traffic allowed in the displayed output:

```
Output
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere
Nginx HTTP                 ALLOW       Anywhere
OpenSSH (v6)               ALLOW       Anywhere (v6)
Nginx HTTP (v6)            ALLOW       Anywhere (v6)
```

### Check your Web Server

At the end of the installation process, Ubuntu 16.04 starts Nginx. The web server should already be up and running.

We can check with the systemd init system to make sure the service is running by typing:

```
$ systemctl status nginx

Output
● nginx.service - A high performance web server and a reverse proxy server
   Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
   Active: active (running) since Mon 2016-04-18 16:14:00 EDT; 4min 2s ago
 Main PID: 12857 (nginx)
   CGroup: /system.slice/nginx.service
           ├─12857 nginx: master process /usr/sbin/nginx -g daemon on; master_process on
           └─12858 nginx: worker process
```

### Map a Domain To a Service Running on Your VPS with nginx

In this section, you'll learn how to set up a reverse proxy with nginx in a few simple steps.

```
$ sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/test.com
```

Open the new file with sudo privileges in your editor:

```
$ sudo nano /etc/nginx/sites-available/test.com
```

When you are finished, your file will likely look something like this:

```
upstream node-helloworld.your-domain.com {
        server 127.0.0.1:5000;
        keepalive 64;
}

server {
        listen 80;
        server_name helloworld.your-domain.com;

        access_log /var/log/nginx/helloworld.your-domain.com-access.log;
        error_log /var/log/nginx/helloworld.your-domain.com-error.log;

        location / {
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header X-Forwarded-Proto $scheme;
                proxy_set_header Host $http_host;
                proxy_set_header X-NginX-Proxy true;
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection "upgrade";
                proxy_pass http://node-helloworld.your-domain.com;
                proxy_redirect off;
                proxy_http_version 1.1;
                proxy_cache_bypass $http_upgrade;
        }
}
```

When you are finished, save and close the file.

Now that we have our server block files, we need to enable them. We can do this by creating symbolic links from these files to the sites-enabled directory, which Nginx reads from during startup.

We can create these links by typing:

```
$ sudo ln -s /etc/nginx/sites-available/test.com /etc/nginx/sites-enabled/
```

Note: to be able to reference multiple domains for one Node.js app (like www.example.com and example.com) you need to add the following code to the file /etc/nginx/nginx.conf in the http section:

```
server_names_hash_bucket_size 64;
```

If the DNS changes are propagated, you can point your web browser to your domain and you should see your application running, accessible from the internet.

Next, test to make sure that there are no syntax errors in any of your Nginx files:

```
$ sudo nginx -t
```

If no problems were found, restart Nginx to enable your changes:

```
$ sudo systemctl restart nginx
```

Nginx should now be serving both of your domain names.
