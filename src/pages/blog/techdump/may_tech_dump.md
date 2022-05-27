---
setup: |
  import Layout from '../../../layouts/BlogPostLayout.astro';
title: June Tech Dump
description: Tech tools i found interesting
date: 2021-05-15
author: "Gold Ayan"
category: "TechDump"
tags:
  - tech-dump
---

Just a collection of news and tools i learned this month. Comment down below if
you know any other tool

## News

### Photonic Neuromorphic Computing

source: [https://phys.org/news/2021-01-photonics-artificial-intelligence-neuromorphic.html](https://phys.org/news/2021-01-photonics-artificial-intelligence-neuromorphic.html)

- Use light instead of electrons to store and process data
- Normal computer waste around 50% of energy for data transfer from Disk to cache
- AI can exploit this tech to improve performance.

## Tools

<!-- more -->

### Shazam

site: [https://www.shazam.com](https://www.shazam.com)

- You want to know the details of the song you are listening, this website can
  help you
- Available as mobile app too.

### Pagodo - Passive Google Dork

source: [https://www.youtube.com/watch?v=lESeJ3EViCo](https://www.youtube.com/watch?v=lESeJ3EViCo)

#### Basics

- Whenever you need to find something in internet, google is the go to website.
- Google has wealth of information, sometimes finding the required information can be troublesome
- Google dorks can you help you in that scenerio
- try the following in google
  - `site:wikipedia.com maths` you only get results from wikipedia
  - This tells google to get websites from wikipedia related maths not from other sites
- Jouranalist, Security researcher use this method to narrow down the information they want.

#### Advanced Info

- [https://www.exploit-db.com/google-hacking-database](https://www.exploit-db.com/google-hacking-database)
- Find leaked passwords, logs

### Docker as IDE

source: [https://medium.com/@ls12styler/docker-as-an-integrated-development-environment-95bc9b01d2c1](https://medium.com/@ls12styler/docker-as-an-integrated-development-environment-95bc9b01d2c1)

- Your computer cannot be replaced by other computers, due to few things
  - Tools you liked are right around the corner.
  - Configuration on your tools
- If you are developer you can create a docker config of the tools you need and
  install it Whenever you need it.
- Not a new idea, i used **LiveOverflow** docker config replaced few things
  and used it to analyze binary files and solve ctf challenges.
- My docker config: [https://github.com/ThangaAyyanar/Security-Shuriken/blob/master/dockerfile](https://github.com/ThangaAyyanar/Security-Shuriken/blob/master/dockerfile)

### PortMaster

source: [https://github.com/Safing/portmaster](https://github.com/Safing/portmaster)

- Are you worried about what you computer when connected to network. If you use
  Windows or Ubuntu you can try PortMaster to check which application are talking
  via network
- Portmaster is a free and open-source application that puts you back in charge
  over all your computer's network connections

### Proxychains and tor

- Proxychains is proxy within proxy
- Typical VPN

```
Your computer -> Proxy -> Internet
```

- Proxychains

```
Your computer -> Proxy1 -> Proxy2.. -> Proxy(N) -> Internet
```

- You can mix and match proxy of different kinds too.
- More details on [Proxychains github](https://github.com/haad/proxychains)

### Xargs usage

source: [https://www.youtube.com/watch?v=5EFY5ztZb00](https://www.youtube.com/watch?v=5EFY5ztZb00)

- Xargs convert input into `argument list`
- To know what xargs is doing

```
xargs -t
```

- `-n` how much args at time to be passed

```
xargs -t -n 1
xargs -n 1
```

- `-I` where to place the argument

```
ls | xargs -I {} echo {}
```

- How many parallel operation at a time `-P`

```
echo "a b c d" | xargs -n 1 -P <How many parallel operation> bash -c 'sleep 1; echo $0'
echo "a b c d" | xargs -n 1 -P 1 bash -c 'sleep 1; echo $0'
```
