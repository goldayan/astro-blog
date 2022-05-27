---
setup: |
  import Layout from '../../layouts/BlogPostLayout.astro';
title: Reverse Engineering swift binaries
description: swift binary disection
date: 2021-07-31
author: "Gold Ayan"
category: "Coding"
tags:
  - swift
  - radare2
---

Debugging swift in radare2

```sh
r2 -e bin.lang=swift -e bin.demanglecmd=true <program name>
```

| Command | Actions |
| is | List symbol |
| iS | List sections |
| ic | List classes |
| ~.. | less |

```swift
import Foundation

// Checking condition
let test_number = 5
let condition = test_number > 3 ? true : false
```

```asm
69: entry0 (int64_t arg1, int64_t arg2);
│           ; var int64_t var_11h @ rbp-0x11
│           ; var int64_t var_10h @ rbp-0x10
│           ; var int64_t var_4h @ rbp-0x4
│           ; arg int64_t arg1 @ rdi
│           ; arg int64_t arg2 @ rsi
│           0x100001140      55             push rbp                   ; [00] -r-x section size 9213 named 0.__TEXT.__text
│           0x100001141      4889e5         mov rbp, rsp
│           0x100001144      48c705b93500.  mov qword [sym.__s10HelloWorld11test_numberSivp], 5    ; [0x100004708:8]=0
│           0x10000114f      b803000000     mov eax, 3
│           0x100001154      483b05ad3500.  cmp rax, qword [sym.__s10HelloWorld11test_numberSivp]    ; [0x100004708:8]=0
│           0x10000115b      897dfc         mov dword [var_4h], edi    ; arg1
│           0x10000115e      488975f0       mov qword [var_10h], rsi    ; arg2
│       ┌─< 0x100001162      7d07           jge 0x10000116b
│       │   0x100001164      b001           mov al, 1
│       │   0x100001166      8845ef         mov byte [var_11h], al
│      ┌──< 0x100001169      eb09           jmp 0x100001174
│      ││   ; CODE XREF from entry0 @ 0x100001162
│      │└─> 0x10000116b      31c0           xor eax, eax
│      │    0x10000116d      88c1           mov cl, al
│      │    0x10000116f      884def         mov byte [var_11h], cl
│      │┌─< 0x100001172      eb00           jmp 0x100001174
│      ││   ; CODE XREFS from entry0 @ 0x100001169, 0x100001172
│      └└─> 0x100001174      8a45ef         mov al, byte [var_11h]
│           0x100001177      31c9           xor ecx, ecx
│           0x100001179      2401           and al, 1
│           0x10000117b      88058f350000   mov byte [sym.__s10HelloWorld9conditionSbvp], al    ; [0x100004710:1]=0
│           0x100001181      89c8           mov eax, ecx
│           0x100001183      5d             pop rbp
└           0x100001184      c3             ret
```

using if condition

```swift
import Foundation

// Checking condition
let test_number = 5

let condition: Bool
if test_number > 3 {
    condition = true
}
else {
    condition = false
}
```

```asm
 56: entry0 (int64_t arg1, int64_t arg2);
│           ; var int64_t var_10h @ rbp-0x10
│           ; var int64_t var_4h @ rbp-0x4
│           ; arg int64_t arg1 @ rdi
│           ; arg int64_t arg2 @ rsi
│           0x100001150      55             push rbp                   ; [00] -r-x section size 9197 named 0.__TEXT.__text
│           0x100001151      4889e5         mov rbp, rsp
│           0x100001154      48c705a93500.  mov qword [sym.__s10HelloWorld11test_numberSivp], 5    ; [0x100004708:8]=0
│           0x10000115f      b803000000     mov eax, 3
│           0x100001164      483b059d3500.  cmp rax, qword [sym.__s10HelloWorld11test_numberSivp]    ; [0x100004708:8]=0
│           0x10000116b      897dfc         mov dword [var_4h], edi    ; arg1
│           0x10000116e      488975f0       mov qword [var_10h], rsi    ; arg2
│       ┌─< 0x100001172      7d09           jge 0x10000117d
│       │   0x100001174      c60595350000.  mov byte [sym.__s10HelloWorld9conditionSbvp], 1    ; [0x100004710:1]=0
│      ┌──< 0x10000117b      eb07           jmp 0x100001184
│      ││   ; CODE XREF from entry0 @ 0x100001172
│      │└─> 0x10000117d      c6058c350000.  mov byte [sym.__s10HelloWorld9conditionSbvp], 0    ; [0x100004710:1]=0
│      │    ; CODE XREF from entry0 @ 0x10000117b
│      └──> 0x100001184      31c0           xor eax, eax
│           0x100001186      5d             pop rbp
└           0x100001187      c3             ret
            ;-- entry139
```

**strip** command in xcode tools will strip the binaries

swift-reflection-dump - to get the data model structure in the binaries

Reference:

- [r2con2018 - Analyzing Swift Apps With swift-frida and radare2 - by Malte Kraus](https://www.youtube.com/watch?v=yp6E9-h6yYQ)
