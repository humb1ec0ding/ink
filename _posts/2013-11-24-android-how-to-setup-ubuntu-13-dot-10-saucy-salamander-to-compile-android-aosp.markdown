---
layout: post
title: "[Android] 우분투 Ubuntu 13.10 (Saucy Salamander) 에서 안드로이드 소스 ASOP 빌드 환경 구축"
date: 2013-11-24 01:18
comments: true
categories: [tool]
tags: ['android']
---

Xubuntu 13.10 설치 이후에 `AOSP` (Android Open Source Project) source code build 하기 위한 환경 설정.

* [Initializing a Build Environment](http://source.android.com/source/initializing.html)
* [[GUIDE] How to Setup Ubuntu 13.10 Saucy Salamander to Compile Android ROMs](http://forum.xda-developers.com/showthread.php?t=2464683)

XDA 능력자분이 잘 정리해주신 내용이지만 하나하나 실행해보면서 다시 정리해봅니다.

<!--more-->

### [#0] Caution

* 64bit 할 것.
* sudo 해야하는 경우가 아닌 경우 잘 구분할 것.
* JDK5 : Froyo 혹은 그 이전<br>
**JDK6** : Gingerbread 혹은 그 이후. <br>


### [#1] Install JDK

#### 이전 버전 삭제
많은 경우에 java version 이 꼬여서 문제가 되므로, 이러한 문제가 되지 않도록 설치 이전에 이전 버전 JDK 를 삭제하자.

```bash
$ sudo apt-get purge openjdk-\* icedtea-\* icedtea6-\*
```

이전 버전 삭제 후 Java 버전 확인.

```bash
$ java -version
The program 'java' can be found in the following packages:
 * default-jre
 * gcj-4.6-jre-headless
 * gcj-4.7-jre-headless
 * openjdk-7-jre-headless
 * openjdk-6-jre-headless
Try: sudo apt-get install <selected package>
```

#### JDK 설치
Java6 설치

```bash
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java6-installer
```

Java6 설치 후 java 버전 확인

```bash
$ java -version
java version "1.6.0_45"
Java(TM) SE Runtime Environment (build 1.6.0_45-b06)
Java HotSpot(TM) 64-Bit Server VM (build 20.45-b01, mixed mode)
```

### [#2] Installing required packages (Ubuntu 13.10)

빌드를 위한 package 들을 설치. (많군요...)

```bash
sudo apt-get install git-core gnupg flex bison gperf build-essential                         \
zip curl zlib1g-dev zlib1g-dev:i386 libc6-dev lib32ncurses5 lib32z1 lib32bz2-1.0             \
lib32ncurses5-dev x11proto-core-dev libx11-dev:i386 libreadline6-dev:i386 lib32z-dev         \
libgl1-mesa-glx:i386 libgl1-mesa-dev g++-multilib mingw32 tofrodos python-markdown           \
libxml2-utils xsltproc readline-common libreadline6-dev libreadline6 lib32readline-gplv2-dev \
libncurses5-dev lib32readline5 lib32readline6 libreadline-dev libreadline6-dev:i386          \
libreadline6:i386 bzip2 libbz2-dev libbz2-1.0 libghc-bzlib-dev lib32bz2-dev libsdl1.2-dev    \
libesd0-dev squashfs-tools pngcrush schedtool libwxgtk2.8-dev python
```

설치 완료 이후에 다음 symbolic link 수행.
```bash
sudo ln -s /usr/lib/i386-linux-gnu/mesa/libGL.so.1 /usr/lib/i386-linux-gnu/libGL.so
```

이것으로 tool 설치는 완료 ! <br>
다음부터는 소스코드 관련 작업...

### [#3] Setup repo script

AOSP 소스는 git 를 통하여 많은 repository 에 분산되어 운영이 되어서 이를 손쉽게 받을 수 있도록한
repo script 를 이용해서 빌드를 한다고 하네요.
아래 수행하기 이전에 repo script 가 어떻게 생겼는지 잠시 열어봅시다.

```python
#!/usr/bin/env python

## repo default configuration
##
REPO_URL = 'https://gerrit.googlesource.com/git-repo'
REPO_REV = 'stable'

# increment this whenever we make important changes to this script
VERSION = (1, 20)

# increment this if the MAINTAINER_KEYS block is modified
KEYRING_VERSION = (1, 2)
MAINTAINER_KEYS = """
 # Public Key
 Repo Maintainer <repo@android.kernel.org>
 Conley Owens    <cco3@android.com>
"""

T = 'git'                     # our git command
MIN_GIT_VERSION = (1, 7, 2)     # minimum supported git version
repodir = '.repo'               # name of repo's private directory
S_repo = 'repo'                 # special repo repository
S_manifests = 'manifests'       # special manifest repository
REPO_MAIN = S_repo + '/main.py' # main script
MIN_PYTHON_VERSION = (2, 6)     # minimum supported python version

import optparse
import os
import re
import stat
import subprocess
import sys

if sys.version_info[0] == 3:
  import urllib.request
  import urllib.error
else:
  import imp
  import urllib2
  urllib = imp.new_module('urllib')
  urllib.request = urllib2
  urllib.error = urllib2
...


if __name__ == '__main__':
  main(sys.argv[1:])
```

repo 설치 작업.

```bash
mkdir ~/bin
curl http://commondatastorage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
chmod a+x ~/bin/repo
```

환경 변수 PATH 에 `~/bin` 추가하기.

```bash
vim ~/.bashrc
// add the following line at the the bottom.
export PATH=~/bin:$PATH
```

### [#4] Download AOSP source code

AOSP source download 를 위한 directory 를 만들고, 이동.

```bash
mkdir /path/to/aosp_source
cd    /path/to/asop_source
```

### Download source

#### Kitkat 4.4

최근 안드로이드 소스를 역시 4.4 kitkat 를 받아야하겠죠.<br>
[ASOP frameweork git repository](https://android.googlesource.com/platform/manifest.git) 에 `android-4.4_r1` 이후에도
`android-4.4_r1.1`, `android-4.4_r1.2` 등이 올라와있네요.

```bash
repo init -u https://android.googlesource.com/platform/manifest -b android-4.4_r1
repo sync
```

이후 소스를 한참 받고 나서... 완료된다.

```bash
...
Fetching projects: 100% (407/407), done.
Checking out files: 100% (5713/5713), done.out files:  11% (658/5713)
Checking out files: 100% (45873/45873), done.t files:   9% (4352/45873)
Checking out files: 100% (4082/4082), done.out files:  35% (1456/4082)
Checking out files: 100% (4093/4093), done. out files:  46% (1915/4093)
Checking out files: 100% (514/514), done.ng out files:  44% (231/514)
Checking out files: 100% (21512/21512), done.ut files:  38% (8177/21512)
Checking out files: 100% (798/798), done.ng out files:  34% (276/798)
Checking out files: 100% (988/988), done.ng out files:  26% (264/988)
Checking out files: 100% (478/478), done.
Checking out files: 100% (50299/50299), done.ut files:  20% (10235/50299)
Checking out files: 100% (182/182), done.ng out files:   7% (14/182)
Syncing work tree: 100% (407/407), done.
```

### [#5] Development

Build 하기 이전에 개발에 필요한 정보들을 간단하게 참조.<br>
git repository 를 모아서 repo script 사용하고 있으므로 git 대신에 repo command 를 이용한다고 생각하면 될 듯.

* [Developing](http://source.android.com/source/developing.html)

![img](http://source.android.com/images/git-repo-1.png)

### [#6] Build : Configuration

아래 link 에 AOSP build 에 대해서 좀더 자세한 설명이 있네요.

* [A practical approach to the AOSP build system](http://www.jayway.com/2012/10/24/a-practical-approach-to-the-aosp-build-system/)

#### envsetup.sh

`envsetup.sh` 실행하여 환경 설정.

```bash
$ source build/envsetup.sh
	including device/lge/mako/vendorsetup.sh
	including device/lge/hammerhead/vendorsetup.sh
	including device/asus/tilapia/vendorsetup.sh
	including device/asus/grouper/vendorsetup.sh
	including device/asus/flo/vendorsetup.sh
	including device/asus/deb/vendorsetup.sh
	including device/generic/x86/vendorsetup.sh
	including device/generic/mips/vendorsetup.sh
	including device/generic/armv7-a-neon/vendorsetup.sh
	including device/samsung/manta/vendorsetup.sh
	including sdk/bash_completion/adb.bash
```

#### lunch

`lunch` 이용하여 target 설정. (`lunch` 가 오타가 아니죠 ?)

Option 없이 `lunch` 실행해서 build type 을 선택할 수도 있고

```bash
$ lunch

You're building on Linux
Lunch menu... pick a combo:
     1. aosp_arm-eng
     2. aosp_x86-eng
     3. aosp_mips-eng
     4. vbox_x86-eng
     5. aosp_deb-userdebug
     6. aosp_flo-userdebug
     7. full_tilapia-userdebug
     8. mini_armv7a_neon-userdebug
     9. mini_mips-userdebug
     10. mini_x86-userdebug
     11. full_mako-userdebug
     12. full_maguro-userdebug
     13. full_manta-userdebug
     14. full_arndale-userdebug
     15. full_toroplus-userdebug
     16. full_toro-userdebug
     17. full_panda-userdebug

Which would you like? [aosp_arm-eng]
```

직접 option 을 넣어서 `lunch` 실행도 가능함.

```bash
$ lunch aosp_arm-eng
============================================
PLATFORM_VERSION_CODENAME=REL
PLATFORM_VERSION=4.4
TARGET_PRODUCT=aosp_arm
TARGET_BUILD_VARIANT=eng
TARGET_BUILD_TYPE=release
TARGET_BUILD_APPS=
TARGET_ARCH=arm
TARGET_ARCH_VARIANT=armv7-a
TARGET_CPU_VARIANT=generic
HOST_ARCH=x86
HOST_OS=linux
HOST_OS_EXTRA=Linux-3.11.0-13-generic-x86_64-with-Ubuntu-13.10-saucy
HOST_BUILD_TYPE=release
BUILD_ID=KRT16M
OUT_DIR=out
============================================
```

### [#7] Build : Launch New Shell Window

처음에 AOSP build 하는데 계속 빌드 error 가 나오는 거다.

```bash
$ make -j4
...
dalvik/CleanSpec.mk:47: *** missing separator.  Stop.
```

이것 때문에 Ubuntu 를 몇 번을 다시 깔았는지 모른다. Ubuntu, Xubuntu, MintOS, ... 덕분에 다 깔아봤다. :)<br>
저의 경우에는 **model config 을 한 shell 에서 바로 make build 를 하면 위의 error **가 나왔다. 

따라서, model config 을 하고 나면 **반드시 새로운 shell window ** 에서 빌드를 해야한다.<br>
덕분에 [stackoverflow 에 자문자답](http://stackoverflow.com/questions/20412679/dalvik-cleanspec-mk47-missing-separator-stop-error-of-aosp-building/20436563#20436563) 도 해봤다.

Guide 보고 빌드하는데 특별한 말이 없던데 그 많은 사람중에서 model config 하고 그 shell 에서 build 했던 사람이 별로 없었을까 ???<br>
나만 이상한 사람인가... 그것도 신기하지만 매번 빌드 하기 전에 model config 을 하고, make 했던 나도 이상했네. <br>

만약 model config 하지 않고 바로 make 했던 적이 한번이라도 있었다면 빌드 성공한 적도 있었을텐데...<br>
아인슈타인의 명언이 다시 한 번 생각난다... :)

  "같은 일을 계속 반복하면서 다른 결과를 기대하는 것은 미친 짓이다. 
  -알버트 아인슈타인-"

### [#8] Build : Make

[#7] 에서 **새롭게 띄운 shell window** 에서 최종 build 를 위한 make 실행

```bash
$ make -j4
============================================
PLATFORM_VERSION_CODENAME=REL
PLATFORM_VERSION=4.4
TARGET_PRODUCT=full
TARGET_BUILD_VARIANT=eng
TARGET_BUILD_TYPE=release
TARGET_BUILD_APPS=
TARGET_ARCH=arm
TARGET_ARCH_VARIANT=armv7-a
TARGET_CPU_VARIANT=generic
HOST_ARCH=x86
HOST_OS=linux
HOST_OS_EXTRA=Linux-3.11.0-14-generic-x86_64-with-Ubuntu-13.10-saucy
HOST_BUILD_TYPE=release
BUILD_ID=KRT16M
OUT_DIR=out
============================================
including ./abi/cpp/Android.mk ...
including ./art/Android.mk ...
including ./bionic/Android.mk ...
including ./bootable/diskinstaller/Android.mk ...
including ./bootable/recovery/Android.mk ...
including ./build/libs/host/Android.mk ...
including ./build/target/board/Android.mk ...
including ./build/tools/Android.mk ...
including ./cts/Android.mk ...
````