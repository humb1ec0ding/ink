---
layout: post
title: "[study] Assembly type 비교 : 인텔 vs AT&T"
date: 2017-05-03 16:00:00션
description: "Assembly type 비교"
category: study
tags:  [assembly]
---

매번 혼동되는 assemly 의 인텔 포맷과 AT&T 포맷 비교

```
at&t                             intel
movl -4(%ebp, %edx, 4), %eax     mov eax, [ebp-4+edx*4]
movl -4(%ebp), %eax              mov eax, [ebp-4]
movl (%ecx), %edx                mov edx, [ecx]
leal 8(,%eax,4), %eax            lea eax, [eax*4+8]
leal (%eax,%eax,2), %eax         lea eax, [eax*2+eax]
```

<!-- more -->

[c - AT&T vs Intel Syntax and Limitations? - Stack Overflow](http://stackoverflow.com/questions/972602/att-vs-intel-syntax-and-limitations)

## INTEL

```
OP_CODE DST SRC

; 해석  DST = SRC
```

- C 표준 함수와 동일한 방식


## AT&T

```
OP_CDE SRC DST

; 해석  SRC -> DST
```

- 레지스터 이름 앞에는 반드시 `%` 기호가, 숫자 앞에는 `$` 기호가 나옴.


## GDB

```
set disassembly-flavor att
set disassembly-flavor intel
show disassembly-flavor
```


