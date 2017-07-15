---
layout: post
title: "[Android] dalvik/CleanSpec.mk:47: *** missing separator. Stop. AOSP build error"
date: 2013-12-07 11:18:33 +0900
comments: true
categories: etc 
tags: android
---

```
$ make -j4
dalvik/CleanSpec.mk:47: *** missing separator.  Stop.
```
TL;DR Please execute make operation in **the new shell window**, not in the same shell after configuring the model.

<!--more-->

I just build the AOSP just after downloading without changing nothing, but I faced the above error.<br>
I worked for my case.


### [ERROR] Execute make in the same shell window after configuring model.

```
$. build/envsetup.sh 
including device/generic/x86/vendorsetup.sh
including device/generic/mips/vendorsetup.sh
including device/generic/armv7-a-neon/vendorsetup.sh
including device/asus/tilapia/vendorsetup.sh
including device/asus/flo/vendorsetup.sh
including device/asus/grouper/vendorsetup.sh
including device/asus/deb/vendorsetup.sh
including device/samsung/manta/vendorsetup.sh
including device/lge/mako/vendorsetup.sh
including device/lge/hammerhead/vendorsetup.sh
including sdk/bash_completion/adb.bash

$ lunch

You're building on Linux

Lunch menu... pick a combo:
     1. aosp_arm-eng
     2. aosp_x86-eng
     3. aosp_mips-eng
     4. vbox_x86-eng
     5. mini_x86-userdebug
     6. mini_mips-userdebug
     7. mini_armv7a_neon-userdebug
     8. aosp_tilapia-userdebug
     9. aosp_flo-userdebug
     10. aosp_grouper-userdebug
     11. aosp_deb-userdebug
     12. aosp_manta-userdebug
     13. aosp_mako-userdebug
     14. aosp_hammerhead-userdebug

Which would you like? [aosp_arm-eng] 14

============================================
PLATFORM_VERSION_CODENAME=REL
PLATFORM_VERSION=4.4
TARGET_PRODUCT=aosp_hammerhead
TARGET_BUILD_VARIANT=userdebug
TARGET_BUILD_TYPE=release
TARGET_BUILD_APPS=
TARGET_ARCH=arm
TARGET_ARCH_VARIANT=armv7-a-neon
TARGET_CPU_VARIANT=krait
HOST_ARCH=x86
HOST_OS=linux
HOST_OS_EXTRA=Linux-3.11.0-14-generic-x86_64-with-Ubuntu-13.10-saucy
HOST_BUILD_TYPE=release
BUILD_ID=KRT16M
OUT_DIR=out
============================================

$ make -j4
============================================
PLATFORM_VERSION_CODENAME=REL
PLATFORM_VERSION=4.4
TARGET_PRODUCT=aosp_hammerhead
TARGET_BUILD_VARIANT=userdebug
TARGET_BUILD_TYPE=release
TARGET_BUILD_APPS=
TARGET_ARCH=arm
TARGET_ARCH_VARIANT=armv7-a-neon
TARGET_CPU_VARIANT=krait
HOST_ARCH=x86
HOST_OS=linux
HOST_OS_EXTRA=Linux-3.11.0-14-generic-x86_64-with-Ubuntu-13.10-saucy
HOST_BUILD_TYPE=release
BUILD_ID=KRT16M
OUT_DIR=out
============================================
dalvik/CleanSpec.mk:47: *** missing separator.  Stop.
```

### [OK] Execute make in the new shell window.
```
$ . build/envsetup.sh 
$ lunch

// Launch new shell window
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
...
```

It was the my very first time to ask and answer for myself in `stackoverflow`.

* [dalvik/CleanSpec.mk:47: *** missing separator. Stop. error of AOSP building](http://stackoverflow.com/questions/20412679/dalvik-cleanspec-mk47-missing-separator-stop-error-of-aosp-building/20436563#20436563)

