---
layout: post
title: "[study] 64bit machine 에서 GCC 32bit build"
date: 2017-05-03 21:00:00
description: "64bit/32bit GCC 컴파일 옵션"
category: study
tags:  [compile]
---

64bit machine 에서 32bit binary 로 GCC compile 하는 방법

```bash
$ gcc -m32 -o main main.c
```

<!-- more -->

#### [HowTo Compile a 32-bit Application Using gcc On the 64-bit Linux Version – nixCraft](https://www.cyberciti.biz/tips/compile-32bit-application-using-gcc-64-bit-linux.html)

## 64bit : `-m64` option

```bash
$  gcc -o main main.c 

$ file main
main: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 2.6.32, BuildID[sha1]=f52fef90283ad4f65ebaf47716936a3e1ad739e7, not stripped

$ ldd main
        linux-vdso.so.1 =>  (0x00007ffe2f7fb000)
        libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f010d7ab000)
        /lib64/ld-linux-x86-64.so.2 (0x0000557262a74000)
```


## 32bit : `-m32` option 

```bash
$  gcc -m32 -o main32 main.c
```

### Build error : missing package 

그런데, build 하니깐 error 발생.

```bash
$  gcc -m32 -o main32 main.c
In file included from /usr/include/alloca.h:21:0,
                 from main.c:2:
/usr/include/features.h:364:25: fatal error: sys/cdefs.h: No such file or directory
 #  include <sys/cdefs.h>
                         ^
compilation terminated.
```

#### [14.04 - fatal error: sys/cdefs.h: No such file or directory| - Ask Ubuntu](https://askubuntu.com/questions/470796/fatal-error-sys-cdefs-h-no-such-file-or-directory)

```bash
$ sudo apt-get install g++-multilib
```

```bash
$ gcc -m32 -o main32 main.c

$ file main32
main32: ELF 32-bit LSB shared object, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, for GNU/Linux 2.6.32, BuildID[sha1]=67908ef27e6d92c9448f44db2606e33e68852f32, not stripped

$ ldd main32
        linux-gate.so.1 =>  (0xf77f7000)
        libc.so.6 => /lib/i386-linux-gnu/libc.so.6 (0xf7613000)
        /lib/ld-linux.so.2 (0x565ea000)
```

