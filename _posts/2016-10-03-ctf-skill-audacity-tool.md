---
layout: post
title: "[CTF skill] Audacity audio tool"
date: 2016-10-03 08:00:00교
description: "CTF Tool : Audacity 사용법"
category: ctf
tags:  [audacity]
---

이번 TumCTF 2016 에 나왔던 stego 50짜리 문제이다.

<!-- more -->

[TUM CTF 2016 - thejoyofpainting – duksctf – Bunch of security enthusiasts who sometimes play CTF](http://duksctf.github.io/TUMCTF2016-thejoyofpainting/)

```
Stego > thejoyofpainting

Description

listen carefully

Details

Points: 50 Basepoints + 100 Bonuspoints * min(1, 3/137 Solves)
```

`flac` 오디오 파일이 주어지고 숨어있는 flag 를 찾는 문제이다.

[Audacity® is free, open source, cross-platform audio software for multi-track recording and editing.](http://www.audacityteam.org/)

![img](https://raw.githubusercontent.com/humb1ec0ding/humb1ec0ding-etc/master/2016/10/audacity.png)

메뉴에서 **waveform** 선택

![img](https://raw.githubusercontent.com/humb1ec0ding/humb1ec0ding-etc/master/2016/10/audacity_menu.png)

바로 flag 가 보인다. T_T;

![img](https://raw.githubusercontent.com/humb1ec0ding/humb1ec0ding-etc/master/2016/10/audacity_spectrum.png)
