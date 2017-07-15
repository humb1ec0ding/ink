---
layout: post
title: "Mobile & IoT Updates (W23, 2015)"
date: 2015-06-07 13:00
comments: true
categories:  etc
tags: update
---

Mobile 과 IoT 관련 뉴스 중에서 관심있는 내용에 대해서 나름대로 정리하면서 생각해보기...

<!--more-->

## Platform/OS

#### Google OS for IoT : Brillo

* [Google Developing ‘Brillo’ Software for Internet of Things](https://www.theinformation.com/Google-Developing-Brillo-Software-for-Internet-of-Things)
* [Google Brillo OS — New Android-based OS for Internet of Things](http://thehackernews.com/2015/05/Brillo-os-internet-of-things.html)
* [Google developing “Brillo” Internet of Things OS based on Android](http://arstechnica.com/gadgets/2015/05/google-developing-brillo-internet-of-things-os-based-on-android/)

구글의 IoT 를 위한 OS. Android 기반이라고 하며, IoT를 위한 저사양 단말 (32MB, 64MB ~ )을 위한 OS. 제조사에는 무상으로 제공.

	현재 많은 사람들이 IoT 를 이야기하고 있지만 실제 IoT 로부터 부가가치가 나오기 위해서는 Thing, Mobile, Server 가 서로 연동하면서 가치/서비스를 창출해야 한다. 그동안은 각자가 알아서 이를 서로 엮었다고 한다면 WEAVE 를 통한 공통의 언어를 통일해주는 것은 정말 가치있는 일인 것 같다. 이런 일은 구글 정도 아니면 하기 힘든 일이 아닐까한다.	하지만, 안드로이드 학습으로 이제 많은 제조사들이 구글의 오픈소스 플랫폼의 장단점을 너무나도 잘 알게 되었다. 달콤하지만 결코 장기적으로 자신들의 비즈니스가 될 수 없다는 것을... IoT 세계에는 어떻게 대응할까 ?

#### Hawei : LiteOS

* [Huawei’s Agile Network 3.0 Architecture Launched at Huawei Network Congress 2015](http://pr.huawei.com/en/news/hw-432402-agilenetwork3.0.htm#.VVx9UZNVhBe)

Huawei’s Agile Network architecture에 소개된 Agile Internet of Things (IoT) 솔루션 : Agile IoT gateway/Agile Controller/LiteOS, a lightweight IoT operating system (OS). 
ICT 인프라를 표준화해야  IoT 및 인터넷을 발전시키게 될 것임.  사이즈는 10KB 정도. 추가 설정없으며, 자동 탐색 가능하고, 자동 네트워킹 가능.  LiteOS 를 모든 개발자에게 공개. 

**Business-driven ICT Infrastructure (BDII)**  : Hawei 는 인프라 스트럭쳐 제품과 솔루션에 집중하고,  upstream 과 downstream 파트너와 함께 협업하여 산업 어플리케이션 중심의 IoT 에코시스템을 구축한다.

	네트워크 장비 업체인 Hawei 관점에서의 IoT 비즈니스. 인프라에 집중하고, 이를 기반으로 어플리케이션 업체와 협업하여 에코를 키우고자하는 전략이 괜찮아 보인다. 그러나 역시나 가장 문제는 디바이스, 솔루션 개발자 및 업체가 Hawei 를 선택할 것인가 ? 그 해답은 인프라의 편리함과 성숙함에 달려있을 듯.

## H/W, Chipset

#### Samsung : Artik

![samsung artik](http://icdn4.digitaltrends.com/image/artik-1878x670.png)

* [(home) artik.io](https://www.artik.io/media)
* 기사
	- [SAMSUNG SHOWS OFF THE TINY PROCESSORS THAT’LL POWER YOUR NEXT INTERNET-CONNECTED REFRIGERATOR](http://www.digitaltrends.com/mobile/samsung-unveils-artik-platform-for-iot/)

홈, 웨어러블 제품을 위한 경량화된 chipset.  하드웨어 레벨에서 **embedded secure element 내장**


	Embedded secure element 내장한 부분은 좋다.	제조업체로서의 IoT 접근. 하드웨어 제품을 위한 경량화된 chipset 자체개발. 여기에 올라가는 소프트웨어 개발 툴을 오픈하여 개발자 모으고, 에코 확장 시켜나가겠다. 개발자들이 삼성 제품과 플랫폼을 선택해야하는 이유는 뭘까 ?

