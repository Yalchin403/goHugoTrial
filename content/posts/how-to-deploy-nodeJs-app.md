---
title: "How to Deploy NodeJs App"
date: 2022-04-23T20:33:34+04:00
draft: false
cover: "blog-images/deploy-nodejs.webp"
useRelativeCover: true
readingTime: true
tags: ["Node.js", "Deployment"]
---


In this tutorial, we will be deploying a simple URL shortener API built-in express framework of NodeJs. It is pretty simple, uses MongoDB as its database, and you can find its source code: https://yalchin.ml/Cq93RQ.

As a VPS, I use a Linode ubuntu server, and I assume that you have at least some basic knowledge when it comes to Linux.

### Install Node and NPM

```bash
    curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
    
    sudo apt install nodejs
    
    # verify that bash is installed
    node --version
```
### Clone your Repo

```bash
    git clone [link to your github repo]
```

### Install npm dependency packages

```bash
    cd yourproject
    npm install
    npm start (or whatever your start command)
    # stop app
    ctrl+C
```

To be able to run our app in the background continuously, we will be using a package called **PM2**. It is a daemon process manager that will help you manage and keep your application online.

```bash
    sudo npm i pm2 -g
    
    #(name of your appfile can be different)
    pm2 start app
    
    # Other pm2 commands
    pm2 show app
    pm2 status
    pm2 restart app
    pm2 stop app
    pm2 logs (Show log stream)
    pm2 flush (Clear logs)
    
    # To make sure app starts when reboot
    pm2 startup ubuntu
```

You should now be able to access your app using your IP and port. Now we want to set up a firewall blocking that port and set up NGINX as a reverse proxy so we can access it directly using port 80 (HTTP)

```bash
### Set up a ufw firewall

    sudo ufw enable
    sudo ufw status
    sudo ufw allow ssh (Port 22)
    sudo ufw allow http (Port 80)
    sudo ufw allow https (Port 443)
```

### Configure and install NGINX

```bash

    sudo apt install nginx
```

Navigate into

```bash
    cd /etc/nginx/sites-enabled
```

and create a config file for Nginx, name it as your domain. In my case my domain is api.kydev.ml, so I will be naming it as api.kydev.ml.conf . To create a file you can use the touch command or directly open a file with vim, if the file doesn't exist, vim will create a one and open it for you. So use the method below to create:

```bash
    touch api.kydev.ml.conf
    
    # OR
    
    vim api.kydev.ml.conf

```

If you are not familiar with vim, for now, use touch command but for the future, you can take a look at [this cheat sheet](https://yalchin.info/blog/written-tutorial-on-vim/) to learn it.

Open the file you have created with your favorite editor, in my case:

```bash
    sudo vim /etc/nginx/sites-available/api.kydev.ml.conf
```

Add the following snippet, but don't forget to modify it for your needs:

```bash
    server {
    server_name yourdomain.com www.yourdomain.com;
    
        location / {
            proxy_pass http://localhost:5000; #whatever port your app runs on
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection 'upgrade';
            proxy_set_header Host $host;
            proxy_cache_bypass $http_upgrade;
        }
    }

```
### Check NGINX config:

```bash
    # Check NGINX config
    sudo nginx -t
    
    # Restart NGINX
    sudo service nginx restart
```

You should now be able to visit your IP with no port (port 80) and see your app. Now let's add a domain. **Congrats!**

### Add the domain

Add an A record for @ and for www to your droplet, It may take a bit to propagate

### Add SSL with LetsEncrypt

```bash
    sudo add-apt-repository ppa:certbot/certbot
    sudo apt-get update
    sudo apt-get install python-certbot-nginx
    sudo certbot --nginx -d yourdomain.com -d www.yourdomain.com
    
    # Only valid for 90 days, test the renewal process with
    certbot renew --dry-run
```


You should now be able to visit your domain and see your Node app up and running securely!

**Thanks for reading!**