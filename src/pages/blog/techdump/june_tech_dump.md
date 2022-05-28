---
setup: |
  import Layout from '../../../layouts/BlogPostLayout.astro';
title: June Tech Dump
description: Tech news and tools i found interesting
date: 2021-06-28
author: "Gold Ayan"
category: "TechDump"
tags:
  - tech-dump
---

Just a collection of news and tools i learned this month. Comment down below if
you know any other tool.

## Theory

### Actor Model

- Conceptual Model for concurrent computation.
- Originated in 1973

#### What is Actor ?

- Fundamental unit of computation.
- Light weight than thread.
- Isolated from each other.
- Has it own private State.
- No Share memory.
- Updating the private state happens through recieving message.
- Communication happens through messages.
- Each actor has it own mailbox where all messages are recieved.
- Inorder to commnicate between other Actor, The actor need to know the address of Actor.
- Actor can run in single machine or run in mutiple machine
<!-- more -->

#### What Actor can do ?

- Create Another Actor
- Send a message
- Designate How to handle the next message

#### Cons

- Susceptible to deadlocks
- Mailbox overflowing

### Linux Man page

The entire Linux manual collection of pages are traditionally divided into
numbered sections:

- Section 1 : Shell commands and applications
- Section 2 : Basic kernel services – system calls and error codes
- Section 3 : Library information for programmers
- Section 4 : Network services – if TCP/IP or NFS is installed Device drivers and network protocols
- Section 5 : Standard file formats – for example: shows what a tar archive looks like.
- Section 6 : Games
- Section 7 : Miscellaneous files and documents
- Section 8 : System administration and maintenance commands
- Section 9 : Obscure kernel specs and interfaces
- Usage

```
man <section number> <command>
eg:
man 3 printf
man 4 inet
```

## Tools

## Cross Compiler Docker Image

- Cross compiler docker image
- Provide ability to build software for other system from linux.
- Example:
  - You can build binary for Arm64 Linux from x86_64 Linux

[https://github.com/dockcross/dockcross](https://github.com/dockcross/dockcross)

### IPFS - InterPlanetary File System

- Decentralized network similar to Torrent
- When you upload the data to the network you get CID (Content IDentifier)
- Using CID we can access others data.
- We can deploy website in it too.

```
brew install ipfs
```

[https://awesome.ipfs.io/](https://awesome.ipfs.io/)

- Pining service is used to keep your data when you goes offline

### Go chromecast

- Cast video to chrome cast from terminal. Install go-chromecast for mac

```
brew install go-chromecast
```

- List devices in the network and the uuid

```
go-chromecast ls
```

- Load the media file

```
go-chromecast -u <uuid> load <file>
```

- Command line graphics interface to control seek, pause and play.

```
go-chromecast ui
go-chromecast -u <uuid> ui
```

## Barrier - Share keyboard and mouse with other computer

- Install the barrier in the systems you wish to share your keybord and mouse.

```
https://github.com/debauchee/barrier
```

- One system act as server and other act as client
- Start the server
- In server click server configuration and Add new screen
- Then enter the screen name (Exactly same as the client(other computer)
  screen name(screen name can be found in the client barrier software))
- Open the barrier in client and connect to the server by specificing ip
  address and port or it can be done automatically by default.
- Now you can share the keyboard and mouse **Automagically**.
