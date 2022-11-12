---
setup: |
  import Layout from '../../layouts/BlogPostLayout.astro';
title: Playing with Alpine VM
description: playing with alpine linux like transfering files from host to guest, ssh.
date: 2022-11-12
author: "Gold Ayan"
category: "Virtualization"
tags:
  - Alpine
---

Let's say you need to play/learn virtualization on linux but you are system has
pretty low specs then what would you do ?
we would use a low foot print linux distro, I choosed Alpine

Before we start, we need to download the alpine iso, Alpine offers
wide variety of iso. you can download the iso from 
- https://alpinelinux.org/downloads/
I choose virtual(x86_64) and it is only 50MB (Mind Blown).

Okay let's boot up Virtual Box and Create a new VM 
Type   : Linux
Version: Other Linux(64 Bit)
no other special option, just go with default option.

Before booting the server, let's set the networking.

### Networking
- We need to forward the SSH port to Host so that we can connect to SSH using NAT.
- Right click the VM and choose the settigs.
- Navigate to the Network tab.
- Click the Advanced and Tap the Port Forwarding button
- Add new Rule
  - Name: SSH
  - Protocol: TCP
  - Host Protocol: 2200
  - Guest Protocol: 22
- Add another Rule
  - Name: Test
  - Protocol: TCP
  - Host Protocol: 4444
  - Guest Protocol: 4444
- Just leave the host and guest IP address as blank.

### Booting
- Start the VM
- When asked for username enter *root*

### Setting up network
- By default all the network interface are down
- You can check the network interface using
```
ifconfig -a
```
- To up the network interface
```
ifconfig lo up
ifconfig eth0 up
```
- Get IP address automatically
```
vi /etc/network/interfaces
iface eth0 inet dhcp
```
- up the eth0
```
ifup eth0
```
- Now check if you have ip address in eth0.

### Checking network connection
- To check Host and Guest network connection works properly.
- We will use a tool called `nc` (netcat).
- In Alpine enter
```
nc -l -p 4444
```
- In Host system enter
```
nc localhost 4444
```
- if you type in guest/host, the text will appear in other system.
- If that happens then network connection is configured properly.

### Setting up SSH Server
- Install openssh server in alpine
```
apk add openssh
```
- Enable the sshd service so that its starts at boot
```
rc-update add sshd
```
- List services using
```
rc-status
```
- To check the sshd is enabled or not.
- Before we start let us disable the password authentication for SSH.
```
vi /etc/ssh/sshd_config
```
- Search for the following string and remove # before and set yes to no.
```
PasswordAuthentication no
```
- Start sshd service
```
/etc/init.d/sshd start
```

### SSH Connection between Guest and Host
- In Host system we need to run 
```
ssh-keygen
```
- Will use default file name and no passpharse
- This will create a id\_rsa and id\_rsa.pub file
- We need to copy the file id\_rsa.pub to the Alpine VM
- We can use the nc.
```
nc -l -p 4444 > ~/.ssh/authorized_keys # On VM
nc localhost 4444 < ~/.ssh/id_rsa.pub # On Host
```
- After few seconds we can cut the connection using Ctrl+c.
- Ensure the permission of the ssh related files and folder
```
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```
- Restart the SSH server
```
/etc/init.d/sshd restart
```
- Connect from Host
```
ssh -i ~/.ssh/id_rsa.pub root@localhost -p 2200
```
- Voila you logged into Alpine VM from Host system.

The VM is created for demo purpose if you are using it for real
projects, read below heading
 
### Do's and Don't
- Use passpharse when generating ssh-keygen.
- Use ssh-copy-id instead of nc
```
ssh-copy-id -i path/to/certificate -p port username@remote_host
```

In next article we can look into how to communicate between two Alpine VM.

### Resources
- https://wiki.alpinelinux.org/wiki/Setting_up_a_SSH_server
- https://wiki.alpinelinux.org/wiki/Configure_Networking
