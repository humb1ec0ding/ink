---
layout: post
title: "[Android] Unable to import Eclipse project to Android Studio 0.3"
date: 2013-10-28 22:27
comments: true
categories: etc
tags: ['android','app']
---

[Android Studio](http://developer.android.com/sdk/installing/studio.html) 가 좋아서 써보려고 하는데 매번 gradle 에 대해서 잘 몰라서 그런지 어려움이 생긴다.
0.3 버전이 올라와서 버전 업하고, 기존 code 를 import 하는데 역시나 error 가 발생한다.

<!--more-->

	You are using an old, unsupported version of Gradle. Please use version 1.8 or greater.
	Please point to a supported Gradle version in the project's Gradle settings or in the project's Gradle wrapper (if applicable.)

	Consult IDE log for more details (Help | Show Log)

조금 찾아보니 역시 stackoverflow 에 관련된 글이 있다.
### [Unable to import Eclipse project to Android Studio](http://stackoverflow.com/questions/19485981/unable-to-import-eclipse-project-to-android-studio)

위의 글에 자세히 나와있지만 directory path 가 조금 혼동이 있어서 paste 해본다.

#### 1. PROJECT_HOME/gradle/wrapper/gradle-wrapper.properties

```bash
$ cd project_home_directory
$ cat gradle/wrapper/gradle-wrapper.properties 
#Wed Apr 10 15:27:10 PDT 2013
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
distributionUrl=http\://services.gradle.org/distributions/gradle-1.8-bin.zip
```

#### 2. PROJECT_HOME/build.gradle

```bash
buildscript {
  repositories {
    mavenCentral()
  }
  dependencies {
    classpath ‘com.android.tools.build:gradle:0.6.+’
  }
}
```

이렇게 수정하니 일단 import project 는 완료된다.
Gradle 에 대해서 좀더 천천히 살펴봐야 할 듯 하다. 