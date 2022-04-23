---
title: "How to Set Up Supervisor Service"
date: 2022-04-23T22:53:12+04:00
draft: false
toc: false
cover: "blog-images/supervisorctl.jpeg"
useRelativeCover: true
images:
tags:
  - Deploy
  - VPS
  - Linux
  - Bot
---

This tutorial will help you to run any application in the background with the help of "Supervisor". For this tutorial specifically, I'm going to show how we can set up discord.py bot with "Supervisor", however, this is not the limitation. You can set up any application you want with "Supervisor". I will state essential commands, and also will show you an example configuration file, that you can use to set up your application.  
For this tutorial I assume that you are using **Ubuntu Linux as your OS or any other debian based Linux distro**, and you have already installed supervisor in your machine.

**Creating Configutation File**

You configuration file should be located in

```bash
    /etc/supervisor/conf.d
```

So, navigate to the target location and create your configuration file. For my example we can name it as disbot.conf To navigate to /etc/supervisor/conf.d use following command:

```bash
    cd /etc/supervisor/conf.d
```

You can create your configuration file in the terminal by writing following command:

```bash
    touch disbot.conf
```

Make sure name of your configuration file correspons to your project

**Writing Configuration FIle**

Now we are ready to edit our conf file, open up the conf file that you just created with you favorite editor, in my case I'm using vim as file editor, but nano would also be one of the best choices. To open the file:

```bash
    vim disbot.conf
```

Switch to insert mode by pressing i, and you need to provide information as given in the example configuration file below:

```bash
    command=/usr/bin/python3 /home/root/bots/disbot.py
    directory = /home/root/bots
    autostart=true
    autorestart=true
    stderr_logfile=/var/log/disbot/err.log
    stdout_logfile=/var/log/disbot/out.log

```

Let's break these lines of commands one by one:  
So the first command, as it seems from its name, should be a command to run your application, in our case we run our disbot.py as:

```bash
    python3 disbot.py
```

So, we need to specify where python3 is and where our file is to run the application.

Second command is about showing your top level directory which encompasses your application.

Third, and fourth commands is to auto start/restart the service.

Last two lines specify the log files to store any out or err logs.

After you are done with these configuration make sure that you save the file and exit by using :wq (this command will not work if you use other editor) and then go to /var/log/ directory by using following command:

```bash
    cd /var/log
```

And create a folder to store your log files as specified in the conf file. You do not need to create log files, just creating folder specified in the conf file is enough.  
In our case, we specified folder name as disbot so we can go ahead and create one by:

```bash
    mkdir disbot
```

**Activating The Service**

Now you just need to execute following commands, to make sure that supervisor recognize the conf file and will be able to run it without any problem:

```bash
    sudo supervisorctl reread
    sudo supervisorctl update
```

To make sure that service is up and running you can check its status by using the following command:

```bash
    sudo supervisorctl status
```

You will see name of your service that you specified in conf file and it will look something like this:  
  
[![](https://yalchin.info/media/images/supervisor_status.jpg)](https://yalchin.info/media/images/supervisor_status.jpg)

If you set up everything properly, service status should be "running". However, if you see its not running make sure that, you configured your conf file properly, and check your log files to see what the error is.

Following two commands may also be useful for you when you need to restart, start, or stop the service:

Make sure you use name of your own service instead of disbot

```bash
    sudo supervisorctl restart disbot
    sudo supervisorctl start disbot
    sudo supervisorctl stop disbot
```

Thanks for reading!