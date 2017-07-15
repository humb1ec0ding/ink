---
layout: post
title: "[android] AndBug 처음 사용해보기"
date: 2014-04-06 22:05
comments: true
categories: tool
tags: ['android','debug']
---

[AndBug](https://github.com/swdunlop/AndBug) 라는 android debugger 를 알게되어 처음 사용해봅니다. 

<!--more-->

[AndBug -- A Scriptable Android Debugger](https://github.com/swdunlop/AndBug)

   AndBug is a debugger targeting the Android platform's Dalvik virtual machine intended for reverse engineers and developers. It uses the same interfaces as Android's Eclipse debugging plugin, the Java Debug Wire Protocol (JDWP) and Dalvik Debug Monitor (DDM) to permit users to hook Dalvik methods, examine process state, and even perform changes.

   Unlike Google's own Android Software Development Kit debugging tools, AndBug does not require or expect source code. It does, however, require that you have some level of comfort with Python, as it uses a concept of scripted breakpoints, called "hooks", for most nontrivial tasks. (If you just want to dump loaded classes, methods, or threads, there are example scripts for that.)

   -- Scott Dunlop <swdunlop@gmail.com>

**AndBug** 는 debuggable 한 app process 에 대해서 JDWP (Java Debug Wire Protocol) 을 통하여 붙여서 debugging 합니다. 위의 설명에 의하면 debugging 을 위하여 **source code 가 필요없다**고 하며, **hooks** 라고 불리는 **scripted breakpoints** 와 같은 **python** 기술을 많이 사용하고 있는 것 같습니다. 

<!--more-->

Debuggable 하도록 하기 위해서는 `ro.debuggable` 이 `true` 로 설정되도록 debug build 한 바이너리의 경우에는 모든 app process 이며, product build 한 바이너리의 경우에는 개별 앱에서 `android:debuggable=true` 되도록 설정해야 합니다.

## jdwp debuggable process 확인 

```
adb jdwp : list PIDs of processes hosting a JDWP transport

adb jdwp
4152
>adb shell ps |grep 4152
u0_a337   4152  272   900432 77996 ffffffff 00000000 S kr.tkhwang.tkandromonitor
```

Debug build 된 `kr.tkhwang.tkandromonitor` app 으로 디버깅해봅시다.
Broadcasting sniffer app 으로 MainActivity 에 anonymous class 로 broadcasting receiver 가 구현되어 있습니다.


## andbug process 연결 : `-p pid#`

Debuggable process number 를 확인해서 `-p` option 으로 pid 로 process 에 연결합니다.

```
>./andbug
   -- -p, --pid <opt>
      the process to be debugged, by pid or name
   -- -d, --dev <opt>
      the device or emulator to be debugged (see adb)
   -- -s, --src <opt>
      adds a directory where .java or .smali files could be found
      
./andbug shell -p 4152

## AndBug (C) 2011 Scott W. Dunlop <swdunlop@gmail.com>
>> 

```

## Command option

Help 에 나오는 andbug `command` 내용들입니다.

```
## Commands:
   -- class-trace | ct | ctrace <class-path>
      reports calls to dalvik methods associated with a class
   -- classes [<partial class name>]
      lists loaded classes. if no partial class name supplied, list all classes.
   -- dump <class-path> [<method-query>]
      dumps methods using original sources or apktool sources
   -- help [<command>]
      information about how to use andbug
   -- inspect <object-id>
      inspect an object
   -- methods <class-path> [<method-query>]
      lists the methods of a class
   -- shell
      starts the andbug shell with the specified process
   -- source <src-dir>
      adds a source directory for finding files
   -- statics <class-path>
      lists the methods of a class
   -- thread-trace | tt | ttrace <thread-name>
      reports calls to specific thread in the process
   -- threads [<name>] [verbose=<verbose level>]
      lists threads in the process. verbosity: 0 (thread), (1 methods), (2 vars), (3 vars data)
   -- version | v
      Send version request.

## Examples:
   -- andbug classes -p com.ioactive.decoy
   -- andbug methods -p com.ioactive.decoy com.ioactive.decoy.DecoyActivity onInit     
```

### classes

`classes` 확인

```
>> help classes
## classes [<partial class name>]
   lists loaded classes. if no partial class name supplied, list all classes.
   
>> classes kr.tkhwang.tkandromonitor
## Loaded Classes
   -- kr.tkhwang.tkandromonitor.MainActivity$1
   -- kr.tkhwang.tkandromonitor.MainActivity$2
   -- kr.tkhwang.tkandromonitor.MainActivity
   -- kr.tkhwang.tkandromonitor.MonitorIntents
```

### methods

`methods` 확인 

```
>> help methods
## methods <class-path> [<method-query>]
   lists the methods of a class

>> methods kr.tkhwang.tkandromonitor.MainActivity$1
## Methods Lkr/tkhwang/tkandromonitor/MainActivity$1;
   -- kr.tkhwang.tkandromonitor.MainActivity$1.<init>(Lkr/tkhwang/tkandromonitor/MainActivity;)V
   -- kr.tkhwang.tkandromonitor.MainActivity$1.onReceive(Landroid/content/Context;Landroid/content/Intent;)V
```

### class-trace

class 이름으로 trace 를 걸 수 있는 option 인 것 같습니다. `kr.tkhwang.tkandromonitor.MainActivity$1` anonymous broadcasting receiver class 에 trace 를 걸었습니다.

```
>> class-trace kr.tkhwang.tkandromonitor.MainActivity$1
## Setting Hooks
   -- Hooked kr.tkhwang.tkandromonitor.MainActivity$1 
```

이후에 broadcasting intent 가 날라와서 `onReceive` 가 불리는 경우에는 그 callstack 이 아래와 같이 trace 가 됩니다. 

```java
## trace thread <1> main        (running suspended)
   -- kr.tkhwang.tkandromonitor.MainActivity$1.onReceive(Landroid/content/Context;Landroid/content/Intent;)V:0
      -- this=Lkr/tkhwang/tkandromonitor/MainActivity$1; <830045447480>
      -- intent=Landroid/content/Intent; <830046271632>
      -- context=Lkr/tkhwang/tkandromonitor/MainActivity; <830045443216>
   -- android.app.LoadedApk$ReceiverDispatcher$Args.run()V:67
      -- ordered=True
      -- cl=Ldalvik/system/PathClassLoader; <830045434264>
      -- this=Landroid/app/LoadedApk$ReceiverDispatcher$Args; <830046271920>
      -- mgr=Landroid/app/ActivityManagerProxy; <830045414960>
      -- intent=Landroid/content/Intent; <830046271632>
      -- receiver=Lkr/tkhwang/tkandromonitor/MainActivity$1; <830045447480>
   -- android.os.Handler.handleCallback(Landroid/os/Message;)V:2
      -- message=Landroid/os/Message; <830046247320>
   -- android.os.Handler.dispatchMessage(Landroid/os/Message;)V:4
      -- this=Landroid/app/ActivityThread$H; <830045413376>
      -- msg=Landroid/os/Message; <830046247320>
   -- android.os.Looper.loop()V:377
      -- me=Landroid/os/Looper; <830045412968>
      -- msg=Landroid/os/Message; <830046247320>
      -- ident=44397076942904
      -- logging=None
      -- queue=Landroid/os/MessageQueue; <830045412992>
   -- android.app.ActivityThread.main([Ljava/lang/String;)V:56
      -- args=[]
      -- thread=Landroid/app/ActivityThread; <830045413104>
   -- java.lang.reflect.Method.invokeNative(Ljava/lang/Object;[Ljava/lang/Object;Ljava/lang/Class;[Ljava/lang/Class;Ljava/lang/Class;IZ)Ljava/lang/Object;
      <native>
   -- java.lang.reflect.Method.invoke(Ljava/lang/Object;[Ljava/lang/Object;)Ljava/lang/Object;:17
      -- this=Ljava/lang/reflect/Method; <830045399392>
      -- args=([],)
      -- receiver=None
   -- com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run()V:11
      -- this=Lcom/android/internal/os/ZygoteInit$MethodAndArgsCaller; <830045399784>
   -- com.android.internal.os.ZygoteInit.main([Ljava/lang/String;)V:85
      -- caller=Lcom/android/internal/os/ZygoteInit$MethodAndArgsCaller; <830045399784>
      -- argv=(#'com.android.internal.os.ZygoteInit', #'start-system-server')
   -- dalvik.system.NativeStart.main([Ljava/lang/String;)V <native>
```

### method-trace

method 로 trace 하는 방법.

```java
>> method-trace kr.tkhwang.tkandromonitor.MainActivity.RibbonMenuItemClick
## Setting Hooks
   -- Hooked kr.tkhwang.tkandromonitor.MainActivity.RibbonMenuItemClick(I)V:0
>> ## trace thread <1> main        (running suspended)
   -- kr.tkhwang.tkandromonitor.MainActivity.RibbonMenuItemClick(I)V:0
      -- this=Lkr/tkhwang/tkandromonitor/MainActivity; <830045443216>
      -- itemId=2131296264
```

```java
>> method-trace kr.tkhwang.tkandromonitor.MainActivity.processIntent
## Setting Hooks
   -- Hooked kr.tkhwang.tkandromonitor.MainActivity.processIntent(Landroid/content/Context;Landroid/content/Intent;)V:0

## trace thread <1> main        (running suspended)
   -- kr.tkhwang.tkandromonitor.MainActivity.processIntent(Landroid/content/Context;Landroid/content/Intent;)V:0
      -- this=Lkr/tkhwang/tkandromonitor/MainActivity; <830045443216>
      -- intent=Landroid/content/Intent; <830045593632>
      -- context=Lkr/tkhwang/tkandromonitor/MainActivity; <830045443216>
## trace thread <1> main        (running suspended)
   -- kr.tkhwang.tkandromonitor.MainActivity.processIntent(Landroid/content/Context;Landroid/content/Intent;)V:0
      -- this=Lkr/tkhwang/tkandromonitor/MainActivity; <830045443216>
      -- intent=Landroid/content/Intent; <830045678840>
      -- context=Lkr/tkhwang/tkandromonitor/MainActivity; <830045443216>
```

### break

```java
>> break kr.tkhwang.tkandromonitor.MainActivity$1
## Setting Hooks
   -- Hooked <536870913> kr.tkhwang.tkandromonitor.MainActivity$1 <class 'andbug.vm.Class'>
>> ## Breakpoint hit in thread <1> main    (running suspended), process suspended.
   -- kr.tkhwang.tkandromonitor.MainActivity$1.onReceive(Landroid/content/Context;Landroid/content/Intent;)V:0
   -- android.app.LoadedApk$ReceiverDispatcher$Args.run()V:67
   -- android.os.Handler.handleCallback(Landroid/os/Message;)V:2
   -- android.os.Handler.dispatchMessage(Landroid/os/Message;)V:4
   -- android.os.Looper.loop()V:377
   -- android.app.ActivityThread.main([Ljava/lang/String;)V:56
   -- java.lang.reflect.Method.invokeNative(Ljava/lang/Object;[Ljava/lang/Object;Ljava/lang/Class;[Ljava/lang/Class;Ljava/lang/Class;IZ)Ljava/lang/Object;
      <native>
   -- java.lang.reflect.Method.invoke(Ljava/lang/Object;[Ljava/lang/Object;)Ljava/lang/Object;:17
   -- com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run()V:11
   -- com.android.internal.os.ZygoteInit.main([Ljava/lang/String;)V:85
   -- dalvik.system.NativeStart.main([Ljava/lang/String;)V <native>
```

## Futher...

Class, method 에 대해서 break 를 걸고, callstack 확인을 하는 것까지는 간단하게 확인하였네요. Debugger 를 제대로 사용하기 위해서는 그 경우에 여러가지 변수 값을 확인하고, 여러가지 debugging 을 할 수 있어야 하겠죠. 

아래 command 좀더 활용해보면서 요긴한지 좀더 써봐야 할 것 같네요.

```bash
## Commands:
   -- class-trace | ct | ctrace <class-path>
      reports calls to dalvik methods associated with a class
   -- classes [<partial class name>]
      lists loaded classes. if no partial class name supplied, list all classes.
   -- dump <class-path> [<method-query>]
      dumps methods using original sources or apktool sources
   -- help [<command>]
      information about how to use andbug
   -- inspect <object-id>
      inspect an object
   -- methods <class-path> [<method-query>]
      lists the methods of a class
   -- shell
      starts the andbug shell with the specified process
   -- source <src-dir>
      adds a source directory for finding files
   -- statics <class-path>
      lists the methods of a class
   -- thread-trace | tt | ttrace <thread-name>
      reports calls to specific thread in the process
   -- threads [<name>] [verbose=<verbose level>]
      lists threads in the process. verbosity: 0 (thread), (1 methods), (2 vars), (3 vars data)
   -- version | v
      Send version request.

## Examples:
   -- andbug classes -p com.ioactive.decoy
   -- andbug methods -p com.ioactive.decoy com.ioactive.decoy.DecoyActivity onInit     
``` 