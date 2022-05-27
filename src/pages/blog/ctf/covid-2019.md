---
setup: |
  import Layout from '../../../layouts/BlogPostLayout.astro';
title: Covid19 CTF writeup
date: 2020-05-02
description: CTF writeup
author: "Gold Ayan"
category: "CTF"
tags:
  - ctf
  - writeup
---

CTF Writeup for [Covid19](https://covid19.threatsims.com).

## Covid Scammers

They have given binary file (ELF 32 bit) need to answer the below questions

I analyzed the binary in both **radare** and **ghidra**. Decompilation in ghidra is terrible, but based on radare i double checked the results for 5,6,8.

This challenge has 12 challenge, i managed to solve few

<!-- more -->

### 2. Arch

What architecture is this sample compiled for?

```
x86
```

**How i found it ?**

```
file <binary file>
```

### 3. Who Me? [Not confirmed]

What is this malware sample called (not the actual binary name)?

```
goHct1tkuuvjiey0w9B
```

**How i found it ?**

- You can find the string in the ltrace image (marked as 3)
- This is my guess, i cannot able to check if this is original answer

### 4. Scouting

What is the C2 server? Provide the domain as the answer.

```
covidfunds.net
```

**How i found it ?**

- you can find the ltrace image (marked as 4)

### 5. This is nice, might stay a while...

How does the malware persist? SHA1 hash the path of the persistence location.

echo -n "/full/path" | sha1sum

```
/etc/init.d/zorr
```

**How i found it ?**

- you can find the ltrace image (marked as 5)

### 6. License and Registration Please

The malware creates a UUID and stores it in a file, what is the name of this file.
Provide the SHA1 hash of the full path as the flag.

```
/tmp/.serverauth.tn6aUcM0uM
```

**How i found it ?**

- you can find the ltrace image (marked as 6)

### 8. Shared Secrets [Not confirmed]

The malware creates a shared-memory object and stores a flag inside. Recover the flag.

```
covid{kEepItSeCrETmR.Fr0dO!}
```

**How i found it ?**

- you can find the ltrace image (marked as 8)
- This is my guess, i cannot able to check if this is original answer

![covidctf-covidscammer](/image/covid-ctf-2019/covidscammer.png)

**Things i learned from this challenge**

- i never seen \_i686.get_pc_thunk.cx in reversing binary, which is used in PIC (position independent code)

## Tom Nook - Internet traffic - Part I

- We have a .pcap file
- when i analyzed the **protocol hierarchy**
- it contains tcp,ssh,http
- when i checked the first tcp stream i got the flag

![covidctf-wireshark](/image/covid-ctf-2019/wireshark.png)

## Resources

- \_i686.get_pc_thunk.cx [https://www.technovelty.org/linux/plt-and-got-the-key-to-code-sharing-and-dynamic-libraries.html](https://www.technovelty.org/linux/plt-and-got-the-key-to-code-sharing-and-dynamic-libraries.html)
