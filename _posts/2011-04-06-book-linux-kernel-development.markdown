---
layout: post
title: "[book] Linux Kernel Development (Robert Love)"
date: 2011-04-06 01:42:41 +0900
comments: true
categories: book
tags: linux
---

![](https://raw2.github.com/tkhwang/tkhwang-etc/master/images/linux_kernel_development.jpg)

Linux kernel 에 대해서 좀더 자세하게 알고 싶어서 linux source code 를 download 받고, kernel 관련된 여러가지 책을 찾아 보게 됩니다. 그런데, 처음 linux kernel 에 대해 공부하기 시작하면 그 방대함에 너무 커서 좀처럼 제대로 시작을 하는 것이 쉽지 않습니다.

Bible 이라고 하는 Understanding Linux Kernel 이나 Linux kernel architecture 와 같은 책이 아무리 좋다고 하더라도처음 linux kernel 에 대해서 공부하시는 분이 처음 입문하여 이러한 책들을 보고 이해하는 것은 아무래도 쉽지 않습니다.

<!--more-->

Kernel 하면 scheduler 지... 하고 scheduling 관련 부분 보려고 하면 process 부터 시작하여 runqueue, bitmap, O(1), CFS 등등 엄청나고... Memory management 부분을 알고 싶어서 찾아본다면 역시 physical page 부터 시작하여 buddy allocator, slab allocator, page cache, swap... Driver model 이 좋다고 하던데... 어디 한 번 볼까... 무엇을 어디부터 보아야 하는지 파악하는 것 자체가 쉽지 않습니다.

Robert love  의 Linux kernel development 는 이렇게 방대한 linux kernel 입문서로 가장 좋은 책이 아닐까 싶습니다. 유명한 Robert Love 가 쓴 책으로서 linux 전반적인 부분에 대해서 중요한 부분에 대해서 잘 설명을 한 책입니다. 최근에 3rd edition 이 나왔더군요.

물론 linux kernel 을 이 책 한번을 보고 전부 이해하기를 기대하는 것은 너무 야무진 꿈이겠죠... 이책과 kernel source code 로부터 시작하는 것입니다.... :)

* 1st : [Linux Kernel Development (Robert Love)](http://www.amazon.com/Linux-Kernel-Development-Robert-Love/dp/0672329468/ref=sr_1_1?ie=UTF8&qid=1302095452&sr=8-1)
* 2nd : [Understanding the Linux Kernel (Bovet & Cesati)](http://www.amazon.com/Understanding-Linux-Kernel-Third-Daniel/dp/0596005652/ref=sr_1_1?ie=UTF8&s=books&qid=1302095478&sr=1-1)
* 3rd : [Professional Linux Kernel Architecture (Wolfgang Mauerer)](http://www.amazon.com/Professional-Linux-Kernel-Architecture-Programmer/dp/0470343435/ref=pd_sim_b_6)
 