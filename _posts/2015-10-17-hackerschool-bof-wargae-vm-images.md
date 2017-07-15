---
layout: post
title: "해커스쿨의 Buffer Overflow 및 시스템 해킹 Wargame를 위한 image 모음"
date: 2015-10-18 20:00
comments: true
categories:  wargame
tags: [hacking]
---

보안 취약점을 이용하여 공격 exploit 에 가장 기본이 되는 **buffer overflow 취약점 공격**에 쉬운 설명과 실습을 해볼 수 있는 vm image 환경이 [해커스쿨](http://www.hackerschool.org/Sub_Html/HS_Community/index.html)에서 제공해주는 것들이 있어서 몇 가지 정리해보았습니다. 

<!--more-->

## 실습용 vmware 이미지들

### 초급 : [해커스쿨 시스템 해킹 강좌 : buffer overflow 왕기초편](http://www.hackerschool.org/Sub_Html/HS_University/bof_1.html)

- 실습용 vmware 이미지 : [다운로드] (http://bit.ly/ntat42)
	- 계정 : `student`/`student`,  `root`/`hackerschool`
- [강좌 PDF 링크](http://www.hackerschool.org/Sub_Html/HS_University/bof_1.html)

### 중급 : [해커스쿨 BoF 원정대](http://www.hackerschool.org/HS_Boards/zboard.php?id=HS_Notice&no=1170881885)

- 실습용 vmware 이미지 : [다운로드](http://hackerschool.org/TheLordofBOF/TheLordOfTheBOF_redhat_bootable.zip)
	- 계정 : `gate`/`gate`
- 문제 풀이 모음
	- [inhack](http://inhack.org/wordpress/?cat=69&paged=2) 
	- [BOF 원정대 간단풀이](https://docs.google.com/document/d/1DaTxDdOsvZk39LLdO6qLoywEbiNAHU32nKajpdxX_Wg/edit)
	- [BOF 원정대 풀이(전체) by hyunmini](http://hyunmini.tistory.com/56)

### 고급 : 시스템 해킹 [해커스쿨 Wargame FTZ 로컬 구축](http://moaimoai.tistory.com/46)

- 실습용 vmware 이미지 : [다운로드](http://www.hackerschool.org/Sub_Html/HS_Service/VmwareLinux/Red_Hat_9.0.zip)
- FTZ.iso : [다운로드](https://www.dropbox.com/s/220sa4lx5tzjo89/ftz.iso?dl=1) - vmware image 의 cd-rom 에 mount 
- [환경설정](http://moaimoai.tistory.com/46)
- 책 [문제 풀이로 배우는 시스템 해킹 테크닉- 해커스쿨 FTZ를 활용한 단계별 해킹 수련법](http://www.aladin.co.kr/shop/wproduct.aspx?ItemId=23613347)

## 공통 사용법

- vmware 동작하도록 환경 구축
- `netconfig` 혹은 `/sbin/netconfig` 이용하여 dhcp network 설정 ==> IP 확인.
- 로컬 로긴하거나 `telnet` 접속 이용.

### 한글 깨짐 : encoding  설정

Mac 에서는 기본 UTF-8 환경인데 위의 이미지는 `euc-kr` 한글 encoding 을 사용하고 있다.  따라서 이용 중인 터미널 설정에서 **encoding을 eur-kr** 로 변경해야 한글이 깨지지 않고 보인다. 

평소 iTerm 을 사용하는데 prompt 에 git status 표시하도록 설정을 해두어서 iTerm 에서 encoding 을 변경하여 새로운 창을 띄우면 한글 입력에 문제가 된다. 여러가지 시도해본 결과 [SecureCRT](https://www.vandyke.com/download/securecrt/download.html) 툴이 가장 좋은 듯 하다. ( [Zoc](http://www.emtec.com/zoc/)이 더 좋은 듯한데, **EUC-KR** encoding 이 안 되는 듯.)

## 하나하나 해보자

매번 자세하게 해보지는 않으면서 왜 나는 잘 안 될까 투덜거리고 있는 것은 아닐까 한다. 이제 다시 처음부터 하나씩 하나씩 내손으로 직접 해보면서 기초를 쌓아 나가보려고 한다. 






