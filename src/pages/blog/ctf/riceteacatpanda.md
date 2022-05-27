---
setup: |
  import Layout from '../../../layouts/BlogPostLayout.astro';
title: Rice Tea Cat Panda CTF Writeup
description: CTF writeup
date: 2020-05-01
author: "Gold Ayan"
category: "Coding"
tags:
  - ctf
  - writeup
---

writeup for rtcp that are solved

## Cryptography

### HOOOOOOOOOOMEEEEEE RUNNNNNNNNNNNNN!!!!!

```
AND JAKE IS ROUNDING THE BASES
HE PASSES BASE 32!!!
HE ROUNDS BASE 64!!!!!!!
WE'RE WITNESSING A MIRACLE!!!!!!!!!!!!!

Ecbf1HZ_kd8jR5K?[";(7;aJp?[4>J?Slk3<+n'pF]W^,F>._lB/=r
```

<!-- more -->

I think it is some sort of base encoding so few base encoding 32,64,85

base 85 decoding works

```
rtcp{uH_JAk3_w3REn't_y0u_4t_Th3_uWust0r4g3}
```

#### Tool used

- [https://gchq.github.io/CyberChef](https://gchq.github.io/CyberChef)

## Forensics

### BTS-Crazed

- We have given mp3 file
- **strings** tool on mp3 and searched for string start with rtcp and found the flag

```
rtcp{j^cks0n_3ats_r1c3}
```

### Chugalug's Footpads

- we are given with two images left.jpg and right.png.
- comparing two images in **vimdiff** in hex mode.

you got the flag

```
rtcp{Th3ze_^r3_n0TcH4nC1a5}
```

## Binary/Excecutable

### Tea Clicker

It can be solved using **cheat engine**

## Reverse Engineering

### What's the Password: Revisited

we got a elf file. i used ghidra to reverse engineer it but my ghidra version
doesn't work well but i worked fine for my friend bizzare

```python
ipdata = ['0x3A','0x31','0x52','0x30','0x34','0x36','0x52','0x30','0x33','0x5C','0x3A','0x51','0x73','0x30','0x35','0x45','0x5C','0x31','0x5A','0x34','0x80']

print(len(ipdata))

for data in ipdata:

    asciistr = int(data,16)
    asciino = (asciistr ^ 50) - 1 ^ 50
    print(chr(asciino),end='')
```

the flag is

```
rtcp{50m371m32_5Pr34D_0U7}
```

## General Skills

### Ghost-in-the-system

- The binary is a c++ program
- Reversing the main function in ghidra gives the that it is taking data from a array with index to construct the string
- we copied the array and indices and do the same logic in python we obtain the flag

```python

flagdata="s}yvzezqr_6x45jx2yp4d38qq1mvnsl0u7w32lr12gi}t3i5kw0oewkqb_vv6726}}95cmfy_jfgyx25n1e9cuyvsor_0mijcnhoa2kpvdtjd9js2kstbe5}s6zgyil6qxtr}wbol}dzmg3t02466hu1gkpm2xv8u{ryn0s11uzu_426p8k4owb21f3buof6ok{cp9s2s88k3yhzdsq1d2u7n3}9ex}9sly0p0}lp5yxdi7m37_p82o54im1z7bw5u2tu9n2loybmr51jih8lxf7z6n62goh3_63cnnbfczhmsy4pe}ijluq9xbkk4d{c13s5hjkjldeww9z}78oyt1pog5qudz{6fkw_wgon99yc{7v4sakj6pddk5i1c_1g74e_xwivk7mmbm16it6zxfc1y6sdz{0zrmuvysbl}pmw8z6jb8ejmrqknxbu5w4sv542plnzs8_}znyq6b6x67ar0lsq04qu742uenp4ufoxz7ir8gzohi352}7{9hk{yu4_zbj7gmvl{c_24weh8rwxp_24dhp{giv9k}gz840uezqk9s}qxi{2u2lbbt4i}kq8gomrqewvrj65dgwaoitc99yh4jest6sccnz2wlgmap6f9k04lhanc3wmgpj6xawln_jce6c6vfttu{zws4odom7{h5hewr_{5}6fty4a14ar64q1vvg0s28zsik}nhpmw}j92s42k}zzxx0bn7cddk70iw4{f8wqguyj6a58s0u2}xzwh{0vdawdge8n88j6ms8uvt_r4hezvei3u2k179tlepun{c1l02_e92ijk9xx0o_a8gwnmp1jr9gtk2{cq7qnmrphvyecps}63cqvxcy{i5}d2r1r{rg1n}nufm7sue378uwdqe9ezscxoq90nme76}jx4}}b8ahe_paby2qxqwop63kc6eujs7}f90pkkiddlvfobb24wj52wzu2cnhoa_p4jjw4nh9kr5gif04ojbh1e_eec12"

indices = ['0x1f0','0x192','0x322','0xe4','0x2f6','0x2f4','0x276','0x27a','0x2d6','0xce','0x8e','0x33d','0x1cb','0xd3','0x126','0x22a','0x125','0x2ad','0x1ab','0x274','0x117','0x3a2','0x1cb','0x161','0xfb','0x28a','0xa9','0x216','0x142','0x2f','0x21a','0x325','0x216','0x164','0x1e4','0x1b9','0x2df','0x1e9','0x3d','0x1fe','0x2d3','0x1a1','0x97','0xaa','0x123','0x2d8','0x184','0x2d7','0x90','0x14','0xd9','0x5b','0x346','0x25c','0x1f7','0x1b6','0xba','0x1bd','0x204','0xf8','0x135','0x14c','0xb3','0x35b','0xe9','9','0x1c3','0x333','0x3a1','0xce','0x2f7','0x4f','0x374','0x164','0xb8','0x2c1','0x1cb','0x36f','0x62','0x123','0x395','0x4f','0x12e','0x1bd','0x208','0x142','0xef','0x1cb','0x368','0x2ee','0xd','0x28a','0x3e2','0x388','0x39a','0x3a5','0xf2','0xe5','0x2f7','0xe6']

for index in indices:
    int_index = int(index,16)
    print(flagdata[int_index],end='')

```

the flag is

```
rtcp{wh37h3r_1n4n1m473_f16ur35_0r_un4u7h0r1z3d_c0d3_7h3r35_4lw4y5_4_6h057_1n_7h3_5y573m_1056_726_00}
```
