---
layout: post
title: "2014 Secuinside CTF Prequal"
date: 2014-06-02 00:15
comments: true
categories: ctf
tags: [ctf]
---

개인적으로는 rooting 이나 hacking 에 관심은 많이 있었지만 제가 직접적으로 하기보다는 사실 남의 일로만 생각을 해왔다. 그러다가 최근 **CTF(Catch The Flag)** 라는 것을 알게 되었고, 이를 나도 한 해볼까하는 생각을 하다가 날짜가 잘 맞아서 2014 Secuinside CTF 예선에 재미삼아서 참여를 해보았다.
이전에 CTF 문제를 풀어본적도 없어서 어떤 문제가 나오는지 수준이 어떤지 전혀 몰랐지만 막상 주말 동안 문제를 풀어보니 생각이상으로 쉽지 않더군요.

<!--more-->

* [2014 Secuinside CTF Prequal](http://ctf.secuinside.com/)

![2014 Secuinside CTF Prequal](https://raw.githubusercontent.com/tkhwang/tkhwang-etc/master/images/2014/2014_secuinside_prequal.jpg)

<!--more-->

솔직히 정확하게 문제의 의도가 무엇인지 파악하는 것 자체도 하지 못한 문제가 대부분이고, 특히 **reversing**은 많이 해보지 않아서 그런지 문제가 뭔지도 파악이 어렵더군요. 

그래도 mobile 쪽은 문제 풀 수 있지 않을까하고 내심 기대를 했지만 카카오톡 APK 에 뭔가 장난을 쳤는지 `dex2jar` 로 reversing 이 되지 않은 상황에서 db 암호화된 Key 를 찾아내는 문제였는데 결국 apk reversing 은 실패. 최종 이 문제 해결한 사람이 없어서 어려운 문제였구나 생각은 되지만 전체적인 문제 수준이 쉽지 않은 것을 알게 되었습니다. 

흔희 생각하는 **hacking** 이라기 보다는 **computer system  전반적인 내용에 대한 기발하고 심오한 quiz** 문제 같다는 생각이 듭니다. 물론 그 내용은 주로 hacking 과 관련된 reversing, system, web, network 등 전반적인 분야를 다루고 있는 듯. 

막상 **CTF**를 해보니깐 아주 재미있고, 그동안 업무적으로 해온 것들에 대해서 뭔가 대외적으로 공식적인 평가가 될 수 있을 것 같고 해서 한 번 제대로 해볼 생각입니다. 어차피 관련된 책들은 모두 넘쳐나고 있으니 이번 기회에 좀더 재미있게 한 번 해보려고 합니다. 

우선 `1st Phase` 로서 기본기 다지기 : 각 분야와 관련 책 및 방향성 정리.<br>
(아래 책이 각 분야의 가장 좋은 책인지는 잘 모르겠지만 우선 가지고 있는 책 위주로 정리한 것임.)

## x86 

개인적으로는 mobile 경험 밖에 없어서 ARM 과 Android 와 같은 mobile 경험은 있지만 PC, x86 에 대해서는 아는 내용은 전무. 
사실 컴퓨터의 시작은 x86 인만큼 대세는 x86 인 상황이라서 가장 먼저 준비해야할 분야는 바로 x86 과 reversing 쪽.

* [Practical Reverse Engineering: x86, x64, ARM, Windows Kernel, Reversing Tools, and Obfuscation](http://www.amazon.com/Practical-Reverse-Engineering-Reversing-Obfuscation/dp/1118787315/ref=sr_1_1?s=books&ie=UTF8&qid=1401894394&sr=1-1&keywords=practical+reversing)


## Web Application

Web 쪽도 가장 모르는 분야 중의 하나이므로 먼저 가장 유명한 책으로 기본으로 알아야 할 듯.

* [웹 해킹 & 보안 완벽 가이드](http://www.aladin.co.kr/shop/wproduct.aspx?ISBN=8960770655)
	- 원서 : [The Web Application Hacker's Handbook: Finding and Exploiting Security Flaws](http://www.amazon.com/Web-Application-Hackers-Handbook-Exploiting/dp/1118026470/ref=sr_1_1?s=books&ie=UTF8&qid=1401894492&sr=1-1&keywords=web+application+hackers+handbook)


## 시스템 해킹

시스템 해킹은 가장 관심이 많은 분야이긴 한데 kernel 쪽이라서 어렵고 체계적으로 해본적이 별로 없어서 우선 x86 경험할 겸해서 전체적으로 빨리 경험해보기 위해서 아래 책으로 볼 예정.

* [문제 풀이로 배우는 시스템 해킹 테크닉](http://www.aladin.co.kr/shop/wproduct.aspx?ISBN=8998139146)


## Python

CTF 뿐만 아니라 Pentesting 쪽이 단순하게 기존 tool 만 쓰는 것이 아니라 필요할 경우에 실제로 coding 을 통해서 문제 해결하는 경우가 많은 것 같고, 이 경우 가장 많이 쓰이는 언어가 바로 python 인 듯 하다. Python 으로 pentesting 쪽 몇 몇 케이스에 대해서 code 작성하는 아래 책으로부터 시작해서 python 틈틈히 보려고 함. 

* [Violent Python: A Cookbook for Hackers, Forensic Analysts, Penetration Testers and Security Engineers](http://www.amazon.com/Violent-Python-Cookbook-Penetration-Engineers/dp/1597499579/ref=sr_1_1?s=books&ie=UTF8&qid=1401894692&sr=1-1&keywords=violent+python)

## Penetration testing

Kali linux, backtrack, metasploit 과 같은 툴들은 사실 기본중의 기본이니깐 활요 측면에서 틈틈히 보려고 함. 

* [The Hacker Playbook: Practical Guide To Penetration Testing](http://www.amazon.com/Hacker-Playbook-Practical-Penetration-Testing/dp/1494932636/ref=sr_1_1?s=books&ie=UTF8&qid=1401894780&sr=1-1&keywords=the+hacker+playbook)
* [Basic Security Testing with Kali Linux](http://www.amazon.com/Basic-Security-Testing-Kali-Linux/dp/1494861275/ref=pd_sim_b_3?ie=UTF8&refRID=1DNAWRWCQKN7TJ16V682)


## 읽을거리

해킹 관련된 읽을 거리 책들. 
심심풀이로 틈틈히 지하철에서 볼 예정.

* [더 이상 숨을 곳이 없다- 스노든, NSA, 그리고 감시국](http://www.aladin.co.kr/shop/wproduct.aspx?ISBN=1195038429)
* [해킹, 침입의 드라마](http://www.aladin.co.kr/shop/wproduct.aspx?ISBN=8955508433)
* [네트워크 속의 유령- 신출귀몰 블랙 해커의 사이버 범죄 실화](http://www.aladin.co.kr/shop/wproduct.aspx?ISBN=896077300X)

<br>

기본기 학습한 이후 자아 비판 이후에 다시 `2nd Phase` 계획 예정.<br>
오랜만에 재미있는 목표로서 재미삼아서 한 번 해봐야겠다... 흐흐흐... 
