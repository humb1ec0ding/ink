---
layout: post
title: "[Windows] Windows 에서 분석을 위한 대표적인 analysis tool"
date: 2014-06-06 16:58
comments: true
categories:  tool
tags: hacking
---

Windows 에서 static, dynamic 분석을 위한 대표적인 툴 정리.

<!--more-->

## Static Analysis

* Anti-virus scan  = Virus total
* File Hash = md5Checker
* Packer detection & unpacking = PEiD, stud_PE, upx
* Strings = bintext
* PE File format = PEView, HxD
* DLL & API function = Dependecy walker

## Dynamic Analysis

* File     = Process monitor, AccessEnum, PsFile
* Process  = Process Explorer, Process monitor, Process Hacker, Autoruns, Rersource Hacker
* Registry = Regshot, Process monitor, Autoruns
* Network  = TCPView, Wireshark, Network monitor, TdiMon

<!--more-->

## Static Analysis

### [Anti-virus scan](https://www.virustotal.com/ko/)

악성 코드 판단을 위하여 on-line 에서 여러가지 백신 엔진에서 검진을 제공하는 사이트.

![image](http://dis.um.es/~lopezquesada/documentos/IES_1314/SAD/curso/UT5/ActividadesAlumnos/grupo2/imagenes/virustotal.png)


### [WinMD5Free](https://www.virustotal.com/ko/)

파일의 MD5 hash 를 계산.

![image](http://www.winmd5.com/images/winmd5_screenshot.gif)


### [PEiD](http://www.aldeid.com/wiki/PEiD)

Packer 와 작성된 언어 확인.

![PEiD](http://www.aldeid.com/w/images/c/c6/Peid.png)

* Check a packer or language

### [Stud_PE](http://www.cgsoftlabs.ro/studpe.html)

![image](http://www.downloadsource.net/upload/files/gallery/old/32/3/2/d50782faa00a049984dfb2e4c9fecee3.jpg)


### [BinText](http://www.mcafee.com/kr/downloads/free-tools/bintext.aspx)

바이너리에서 String 문자열 추출.

![image](http://securitylabs.websense.com/images/alerts/bintext_encoder32_2.JPG)

### [PEview](http://wjradburn.com/software/)

Windows PE 파일 분석.

![image](http://althing.cs.dartmouth.edu/local/www.acm.uiuc.edu/sigmil/RevEng/images/PEview.png)

### [Dependency Walker](http://www.dependencywalker.com/)

DLL 의 의존성 조사.

![image](http://www.dependencywalker.com/snapshot.png)


### [UPX : Ultimate Packer for eXecutables](http://upx.sourceforge.net/)

### Hex Editor

#### [HxD](http://mh-nexus.de/en/hxd/)

![image](http://mh-nexus.de/en/graphics/HxDShotLarge.png)


#### [010 editor](http://www.sweetscape.com/010editor/)

![image](http://img.brothersoft.com/screenshots/softimage/0/010_editor-22832-1.jpeg)



## Dynamic Analysis

* File     = Process monitor, AccessEnum, PsFile
* Process  = Process Explorer, Process monitor, Process Hacker, Autoruns, Rersource Hacker
* Registry = Regshot, Process monitor, Autoruns
* Network  = TCPView, Wireshark, Network monitor, TdiMon


### [Process Explorer](http://technet.microsoft.com/ko-kr/sysinternals/bb896653.aspx)

![image](http://4sysops.com/wp-content/uploads/2006/03/Process_Explorer.gif)


### [Process Monitor](http://technet.microsoft.com/ko-kr/sysinternals/bb896645.aspx)

![image](http://www.rarst.net/images/AdvancedtroubleshootingwithProcessMonito_13C05/process_monitor_interface.png)

### [regshot](http://sourceforge.net/projects/regshot/)

두 시점의 파일이나 registry 변경을 비교하기 위하여 snapshot 으로 fie list 를 뽑아서 비교하기 위한 툴.

![image](http://regshot.googlecode.com/files/main_window2.png)

### [Autoruns](http://msdn.microsoft.com/en-us/library/bb963902.aspx)

![image](http://cache.filehippo.com/img/ex/669__autoruns1.png)

### [AccessEnum](http://technet.microsoft.com/ko-kr/sysinternals/bb897332.aspx)

![image](http://screenshots.en.sftcdn.net/en/scrn/33000/33470/accessenum-4.jpg)


