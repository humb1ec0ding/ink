---
layout: post
title: "[blog] Google web font"
date: 2013-10-23 01:01
comments: true
categories: etc
tags: blog
---

어느 경우에나 가장 먼저 하는 것은 theme 과 font 맞추기다.
Octopress blog 라고 해서 절대 예외는 아니다.

현재 무료로 서비스 중인 웹 폰트는 [구글 웹 폰트](http://www.google.com/fonts/earlyaccess)이 가장 많이 쓰이는 것 같지만 나눔 고딕 정도 제공되는 것 같아서 영어에 비해서는 다양하지 못해서 아쉽다. 
혹시나 나중을 위해서 ...

<!--more-->

```html
<link href='http://fonts.googleapis.com/earlyaccess/nanumgothic.css' rel='stylesheet' type='text/css' />
<style type="text/css">
    h1   { font-family: 'Nanum Gothic',Helvetica, Arial, sans-serif;    font-size:20pt;}
    h2   { font-family: 'Nanum Gothic',Helvetica, Arial, sans-serif;    font-size:18pt;}
    body { font-family: 'Nanum Gothic',Helvetica, Arial, sans-serif;  font-size:11pt;}
    p    { font-family: 'Nanum Gothic',Helvetica, Arial, sans-serif;  font-size:11pt;}
</style>
```
