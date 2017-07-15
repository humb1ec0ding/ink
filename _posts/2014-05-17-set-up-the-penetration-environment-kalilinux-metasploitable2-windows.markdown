---
layout: post
title: "kalilinux, metasploitable2, windows 를 이용한 penetration testing 환경 구축"
date: 2014-05-17 11:10
comments: true
categories: tool
tags: ['hacking']
---

최근 penetration testing, white hacking 에 관심을 갖고 제대로 살펴보려 하고 있습니다. 역시나 가장 먼저 출발은 **system 환경 구축**.
White hacking 을 위해서는 kalilinux (backtrack), metasploit 등을 주로 사용하는데, 다음 책에서 virtual 환경으로 kalilinux, metasploitable2, windows 을 가지고 system 꾸미는 내용이 있어서 따라하면서 정리해보았습니다.

<!--more-->

* [Basic Security Testing With Kali Linux](http://www.amazon.com/Basic-Security-Testing-Kali-Linux/dp/1494861275/ref=sr_1_1?s=books&ie=UTF8&qid=1400293211&sr=1-1&keywords=basic+security+testing+with+kali+linux)
* [The Hakcer Playbook](http://www.amazon.com/The-Hacker-Playbook-Practical-Penetration/dp/1494932636/ref=pd_rhf_gw_p_d_8)

<!--more-->

## VMWare Player 를 이용한 가상 환경 구축

기술적으로 Oracle VirtualBox 와 VMWare 의 차이가 무엇인지 잘 모르겠습니다만... Pen testing 환경 구축 시에 VMWare 이용해서 환경을 구축하는 글이 좀더 많은 것 같습니다. System 배포를 VM Image 로 배포하는 경우도 많은 것 같은데, 일단은 책에 따라서 VMWare Player 이용해서 환경 구축해보려고 합니다. 

Host OS 는 Ubuntu 14.04 Linux 와 MAC OS X 을 주로 사용해서 이를 기준으로 작성하였습니다.


### Linux x64
[VMWare Player Download link](https://my.vmware.com/web/vmware/free#desktop_end_user_computing/vmware_player/6_0)

### MAC OS X

[VMWare Fusion 6 for MAC](http://www.vmware.com/products/fusion/)

OS X 에는 VMWare Player 버전이 없고, 대신 VMWare Fusion 을 사용해야고 한다고 합니다. [How do I install VMware player on a Mac os X?](http://superuser.com/questions/456741/how-do-i-install-vmware-player-on-a-mac-os-x)
VMWare Fusion 은 Player 와 달리 freeware 가 아니죠 ?


## Kalilinux

### VM Image download

[Kali Linux VMware Images](http://www.offensive-security.com/kali-llnux-vmware-arm-image-download/)

[Kalilinux download 사이트](http://www.kali.org/downloads/)에서 ISO image 를 받아서 설치해도 되지만 VMWare VM Image 로 배포하여 이를 직접 이용하는 것이 더 편한가 보네요.

VM 이미지라서 x32, x32 PAE, x64 중 어느 이미지를 선택해야하는지 잘 모르겠네요.
Kalinux x64 버전에 약간 문제가 있는 부분이 있다고 들어서 저는 **x32 PAE** 로 다운로드하여 설치하였습니다.

### 설치

VM 에서는 network 환경 설정이 중요한 것 같은데, 우선은 기본 설정인 **NAT** 로 설정.
이후 설치 과정은 그대로 따라하기.

### 업데이트

	apt-get update
    apt-get dist-upgrade

`dist-upgrade` 를 해보면 업데이트하는데 시간이 너무 오래 걸리네요. 
저만 그런 것은 아니고 많이들 발생하는 문제인지 해결책이 적힌 글이 많이 있네요.

* [How to fix Kali Linux apt-get slow update? ](http://www.blackmoreops.com/2013/10/30/fix-kali-linux-apt-get-slow-update/#)

Repository 를 `http.xxx` 를 `repo.xxx` 로 변경하면 된다고 하네요. 많이 빨라 지네요. 

	## Kali Regular repositories	
	deb http://repo.kali.org/kali kali main non-free contrib
	deb http://security.kali.org/kali-security kali/updates main contrib non-free
	## Kali Source repositories
	deb-src http://repo.kali.org/kali kali main non-free contrib
	deb-src http://security.kali.org/kali-security kali/updates main contrib non-free

### VMWare Tools for Linux 설치

메뉴 Virtual Machine > Update VMWare Tools 실행. 


## Metasploitable 2

[Metasploitable 2 download link](http://sourceforge.net/projects/metasploitable/)

[Metasploitable](http://sourceforge.net/projects/metasploitable/)은 exploiting 을 연습하기 위하여 일부러 취약점이 있는 linux OS 이라고 합니다. VM Image 로 배포도 하구요.

## Windows 7

[Cain and Abel](http://www.oxid.it/cain.html)과 같이 windows 에만 있는 tool 들도 있으며, windows 를 좋은 경우도 있으므로 가능하면 windows 도 함께 설치하여 사용할 것을 추천하네요.
Windows 설치는 잘 아시는 방법에 따라서 Windows 설치. 


## The Hacker Playbook 추천 tool 설치

[The Hakcer Playbook](http://www.amazon.com/The-Hacker-Playbook-Practical-Penetration/dp/1494932636/ref=pd_rhf_gw_p_d_8) 에서는 [site](http://thehackerplaybook.com/dashboard/) 를 운영하면서 kalinux 와 windows 에 추가적으로 설치해서 사용하면 좋은 tool list 를 공개하고 있습니다. 
아직은 이 툴들이 얼마나 유용한지 모르겠지만 우선 믿고 설치해봅니다. 

### Kalilinux

Kalilinux 에 추가로 설치할 것을 추천하는 utility list : [https://github.com/macubergeek/gitlist](https://github.com/macubergeek/gitlist)

	mkdir /opt/gitlist/
	cd /opt/gitlist
	git clone https://github.com/macubergeek/gitlist.git
	cd gitlist
	chmod +x gitlist.sh
	./gitlist.sh

### Windows

Windows app 설치는 script 로 자동으로 되지 않아서 그런지 tool list 만 소개하고 있네요.

* [HxD (Hex Editor)](http://mh-nexus.de/en/hxd/)
* [Evade (used for AV Evasion)](http://www.securepla.net/antivirus-now-you-see-me-now-you-dont/)
* [Hyperion](http://nullsecurity.net/tools/binary.html)
* [nmap](http://nmap.org/download.html)
* [oclHashcat](http://hashcat.net/oclhashcat/)
* [Cain and Abel](http://www.oxid.it/cain.html)
* [Burpsuite](http://portswigger.net/burp/)
* [Nishang](https://code.google.com/p/nishang/)
* [Powersploit](https://github.com/mattifestation/PowerSploit)
* Firefox Addons
	- [Wev Develper Add-on](https://addons.mozilla.org/ko/firefox/addon/web-developer/)
	- [Tamper Data](https://addons.mozilla.org/ko/firefox/addon/tamper-data/?src=search)
	- [Foxy Proxy](https://addons.mozilla.org/ko/firefox/addon/foxyproxy-standard/?src=search)
	- [User Agent Switcher](https://addons.mozilla.org/ko/firefox/addon/user-agent-switcher/?src=search)


이제 이렇게 꾸민 환경에서 kalilinux, metasploit 을 중심으로 white hacking 을 조금씩 해보려고 합니다. :)





