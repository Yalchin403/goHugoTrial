---
title: "How to Securely Connect to Virtual Private Server"
date: 2022-04-24T14:26:43+04:00
draft: false
toc: false
cover: "blog-images/vps-hosting-page-banner.jpg"
useRelativeCover: true
images:
tags:
  - VPS
  - Linux
  - Security
---

For your information, this tutorial is not written by me absolutely, I've used ssh connection part of [this](https://notthebe.ee/Creating-your-own-OpenVPN-server.html) tutorial and added some comments to make individual tutorial on "how to create your own vpn" and also covers the things that we can implement to make our vps the way more secure.

**Generating SSH keys**

Using a cleartext password to log in to your server is never a good idea since the password is not encrypted in transit and can be exposed on a hostile network. By creating an SSH key we're going to make it so that you can only log in to the server if you have the key file and the password, and at the same time the password is encrypted. If you're using Linux you probably already know how to open the terminal, if you're on Mac you can find the Terminal app in your Applications folder, and on Windows 10 you'll need to open the PowerShell with administrator's privileges and install SSH using this command:  
 
```sh
    PS C:\> Add-WindowsCapability -Online -Name OpenSSH.Client
```
  
This is the command that will generate our ssh keys. The RSA algorithm with 4096 key size is what I'd personally recommend, since it's sufficiently secure and widely supported.

```sh
    ssh-keygen -t rsa -b 4096
```

Press Enter when asked for the key location to save it to the default one and then enter your password of choice.

**Logging in to the server**

By now our server has started up and we're ready to log in. Copy the IP address from the server control panel, go back to the terminal and type in

```sh
    ssh root@ip-address
```
  
Type **yes**, enter the root password that you specified in the first step and that's it, we're in.

**Updating the OS**

First and foremost, let's update our operating system and software:

```bash
    apt-get update && apt-get upgrade
```

I'll also install my favourite text editor, feel free to use whatever you want though, for example nano.

```bash
    apt install vim
```

**Creating a user**

As much as it's convenient to not have to enter a password every time, we need to create a user account that isn't root. Exposing root login on an SSH server is probably not a good idea even if you have multi factor authentication. Call me paranoid, but I think having to enter root password sometimes is the price I'm willing to pay for some sense of security. Type

```bash
    useradd -G sudo -m wolfgang -s /bin/bash
```

That's going to create a user, set bash as default shell for him and permit sudo usage. Afterwards we'll need to create a password for our user, using

```bash
    passwd yourPassword
```

Enter your password twice and we're good to go.

**Copying SSH key from host to the server**

Now that we've created our user it's a good time to copy the public SSH key to the server. Open a second terminal window for your local terminal and enter:

**Linux or Mac**

```bash
    ssh-copy-id wolfgang@ip_address
```

**Windows**

```sh
    type $env:USERPROFILE\.ssh\id_rsa.pub | ssh ip-address "cat >> .ssh/authorized_keys"
```

You'll be prompted to enter your password and once you do, go back to the terminal window with your server. Don't close the other window yet.

**Restricting SSH to key authentication**

Now that we've copied the SSH keys to the server we have to restrict authentication to the public key only. Let's edit the sshd configuration file

```bash
    nvim /etc/ssh/sshd_config
```

First of all, let's change the default port. This won't do much for security, but it will help with those obnoxious SSH scanners that try to log in with default credentials. Not much, but the security logs will definitely get easier to read. You can use any port that's not taken by other services, but I prefer to use 69. Nice

```sh
    # Port 22 default
    
    Port 69 # make sure that the firewall is open for this port
```

Next, we need to disable password authentication so that you're only able to log in using a public key.

```sh
    PasswordAuthentication no
```

Last but not least, let's also disable root login

```sh
    PermitRootLogin no
```

_If you do so, make sure that the user that you are going to use to log in also has the public key in /home/your\_user\_name/.ssh/authorized_keys file_  
  
Now save the file and restart the sshd service using

```bash
    systemctl restart sshd
```

Now without closing this window let's go back to our local machine and try to log in with our key:

```bash
    ssh -i ~/.ssh/id_rsa wolfgang@ip_address -p 69
```
If you see the prompt to enter your key password, that means we're good to go. It's also a good idea to verify that we can't log in with our password anymore:

```bash
    ssh wolfgang@ip_address -p 69
```
This should give us “Permission denied”.

**Creating a server alias**

But you might have noticed that this command is kind of long and annoying to type, so let's fix that. Create a file in the “.ssh” folder in your home directory called “config” and edit it using your favourite text editor:

```bash
    nvim ~/.ssh/config
```

Here we're going to create an alias for our VPS

```sh
    Host wolfgangsvpn   # choose a name for your server
    User    wolgang    # the username of the user that we created
    Port 69
    IdentityFile ~/.ssh/id_rsa   # that's the location of our key file
    HostName ip_address  # that's the IP address of our server
```

Save and close, and now we can login to our server by simply typing:

```bash
    ssh wolfgangsvpn.
```

If you don't want to see this wall of text every time you login, type in:

```bash
    touch .hushlogin
```


If you do not want to deal with config file, you can use [termius app](https://termius.com/linux) instead

Thanks for reading!