---
setup: |
  import Layout from '../../layouts/BlogPostLayout.astro';
title: Shared Storage Idea
description: Creating a samba server
date: 2021-11-04
author: "Gold Ayan"
category: "Coding"
tags:
  - linux
  - samba
---

Everyone has a smart phone in our house, alteast one smart TV and a one laptop or computer.

Things we do most of the time:

- Take photos in mobile transfer to computer for backup
- Download some video and transfer to Mobile/PC
- Share media or photos with Siblings or Parents
- Play those media in Smart TV

One of the boring task. Can we make it interesting ?

<!-- more -->

We need something that can act as middle man to store the stuff and can be
accessed from any device.

Solutions:

- SMB (Secure Message Block) protocol
- NFS (Network File System)

So let's try to do a POC on SMB. The SMB in linux side called SAMBA (Open source implementation of SMB)

## POC

Installing samba server in Ubuntu

```
sudo apt install samba
```

Configuring samba Configuration

- Tell samba server which folder to share.
- Who have permission to read/write.
- Template of /etc/samba/smb.conf

```
[NAME]
   Configuration
```

- My Config

```
[Public]
    comment = Samba on Ubuntu
    path = /home/sample/Public
    read only = no
    write list = ayan
    browsable = yes
    guest ok = yes

[Videos]
    comment = Samba on Ubuntu
    path = /home/sample/Videos
    browsable = yes
    guest ok = yes
```

- In the above config only user **ayan** has permission to write file in public folder
- For Videos folder no one has write permission.
- Samba shared folders can be accessed by Guest (any one in the network) or Authenticated user.
- Let's see how we can create user for samba authentication.

```
sudo useradd ayan -s /sbin/nologin
```

- The above user creds cannot be used to login in to your linux system because of `-s /sbin/nologin`
- Check if your user added to your system

```
sudo cat /etc/passwd
```

- Set samba password for your user

```
sudo smbpasswd -a ayan
```

- All done
- Restart the samba server

```
sudo service smbd restart
```

### Debugging

- Cheking the service

```
sudo service smbd status
```

- smb config debug

```
sudo testparm
```

## Pros

- All you need is VLC media player software or SMB Client.
- You can play the media in the shared folders.
- If you have SMB client you can copy new files or delete existing files.

One of my favorite quote

> Solution to problem creates new problems

## Cons

- Device should be Alive.
  - We can use _Raspberry PI_ which takes less power and do more than what we need
  - Synology sells hardware that does similar things.
- Devices need to be same network
  - Most of us basically connects to same network (Wifi Router)
  - You can enable hotspot in any device and connect the rest of the device you need to shared.
- Not able to access it through Internet.
  - We can set up software such as Tails or Zeronet through which we can
    access your home server from internet.

## Resources

- Ubuntu official
