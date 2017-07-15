---
layout: post
title: "[bash] cd 명령 후 pwd 자동으로 하기 "
date: 2011-09-24 01:36:06 +0900
comments: true
categories: etc
tags: bash
---
-
bash configuration 을 모아서 git 같은데 개인 설정 모아두던가 해야겠네요...<br>
Ubuntu 자주  사용하게 되면서 예전 설정이 가물가물하네요...

우선 생각나는 것부터 참조 차원에서 정리.

<!--more-->

```bash
// ~/.bashrc 에 아래 내용 추가.
function cd { builtin cd $* && pwd && ls -CF; }
```